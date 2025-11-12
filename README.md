# Tracing Claude Code with Arize

Claude Code의 실행을 Arize 플랫폼으로 추적하고 모니터링하는 프로젝트입니다.

## 📖 개요

이 프로젝트는 Anthropic의 Claude Code CLI 도구의 동작을 Arize AX를 사용하여 추적하고 관찰할 수 있도록 합니다. Claude Code의 API 호출, 응답 시간, 토큰 사용량, 에러 등을 실시간으로 모니터링하고 분석할 수 있습니다.

## ✨ 주요 기능

- **실시간 추적**: Claude Code의 모든 API 호출을 실시간으로 추적
- **성능 모니터링**: 응답 시간, 토큰 사용량, 비용 분석
- **에러 추적**: 실패한 요청과 에러 로그 수집
- **시각화**: Arize AX 대시보드를 통한 직관적인 데이터 시각화
- **디버깅**: 상세한 트레이스 정보로 문제 진단 및 해결

## 🚀 시작하기

### 사전 요구사항

- Python 3.8 이상
- Docker (컨테이너 실행용)
- Claude Code CLI
- Arize AX 무료 계정 ([app.arize.com](https://app.arize.com)에서 가입)
- Alpha Vantage API 키 (무료)
- Supabase 무료 계정 ([supabase.com](https://supabase.com)에서 가입)

### 설치

#### 1. Docker 설치

**macOS 사용자:**
```bash
# Colima와 Docker 설치
brew install colima
brew install docker
```

**Windows 사용자:**
- WSL2 활성화가 필요합니다
- [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/) 설치

**Linux 사용자:**
```bash
# Docker 설치
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

#### 2. 프로젝트 설치

```bash
# 저장소 클론
git clone https://github.com/yourusername/tracing-claude-code-with-arize.git
cd tracing-claude-code-with-arize

# 의존성 설치
pip install -r requirements.txt
```

### 환경 설정

`.env` 파일을 생성하고 필요한 환경 변수를 설정합니다:

```bash
# Anthropic API 키
ANTHROPIC_API_KEY=your_api_key_here

# Arize AX 설정
ARIZE_SPACE_KEY=your_space_key_here
ARIZE_API_KEY=your_arize_api_key_here

# Alpha Vantage API 키
ALPHA_VANTAGE_API_KEY=your_alpha_vantage_key_here

# Supabase 설정
SUPABASE_URL=your_supabase_project_url
SUPABASE_KEY=your_supabase_anon_key
```

#### API 키 발급 방법:

1. **Arize AX API 키**: [app.arize.com](https://app.arize.com)에 로그인한 후, **Settings > API Keys**에서 확인
2. **Alpha Vantage API 키**: [Alpha Vantage](https://www.alphavantage.co/support/#api-key)에서 무료로 발급
3. **Supabase 설정**:
   - [supabase.com](https://supabase.com)에서 프로젝트 생성
   - 프로젝트 대시보드의 **Settings > API**에서 다음 정보 확인:
     - `Project URL` → `SUPABASE_URL`
     - `anon/public key` → `SUPABASE_KEY`

#### API 키 동작 확인

Alpha Vantage API 키를 발급받은 후, 로컬에서 curl로 정상 작동하는지 확인합니다:

```bash
# 샘플 요청 (데모 키 사용)
curl "https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol=IBM&apikey=demo"

# 본인의 API 키로 테스트
curl "https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol=IBM&apikey=YOUR_API_KEY"
```

정상적으로 작동하면 IBM 주식의 현재 시세 정보가 JSON 형식으로 반환됩니다.

## 💻 사용 방법

### 기본 사용

```python
from claude_tracer import ClaudeTracer

# 트레이서 초기화
tracer = ClaudeTracer()

# Claude Code 실행과 함께 추적 시작
tracer.start()

# 여기에 Claude Code 작업 수행
# ...

# 추적 종료
tracer.stop()
```

### Arize AX와 연동

```python
from claude_tracer import ClaudeTracer
from arize.api import Client

# Arize AX 클라이언트 초기화
arize_client = Client(
    space_key='your_space_key_here',
    api_key='your_arize_api_key_here'
)

# Claude 트레이서 초기화
tracer = ClaudeTracer(backend=arize_client)

# 추적 시작
tracer.start()

# Claude Code 작업 수행
# ...

# app.arize.com 대시보드에서 결과 확인
```

### 대시보드 접속

Arize AX 대시보드는 웹 기반으로 제공됩니다:

1. 브라우저에서 [app.arize.com](https://app.arize.com) 접속
2. 로그인 후 프로젝트 선택
3. **Tracing** 탭에서 Claude Code 추적 데이터 확인

## 📊 추적되는 메트릭

- **API 호출 정보**
  - 요청 시간
  - 응답 시간
  - 상태 코드
  - 에러 메시지

- **토큰 사용량**
  - 입력 토큰 수
  - 출력 토큰 수
  - 총 토큰 수
  - 예상 비용

- **성능 지표**
  - 지연 시간 (latency)
  - 처리량 (throughput)
  - 에러율

- **컨텍스트 정보**
  - 프롬프트 내용
  - 모델 설정
  - 도구 사용 내역

## 🔧 고급 설정

### 커스텀 트레이스 필터

```python
# 특정 조건의 트레이스만 기록
tracer = ClaudeTracer(
    filter_fn=lambda trace: trace.duration > 1000  # 1초 이상 소요된 요청만
)
```

### 샘플링 설정

```python
# 모든 요청의 10%만 추적 (프로덕션 환경에서 유용)
tracer = ClaudeTracer(sampling_rate=0.1)
```

## 🤝 기여하기

기여는 언제나 환영합니다! 다음 단계를 따라주세요:

1. 이 저장소를 포크합니다
2. 새로운 기능 브랜치를 생성합니다 (`git checkout -b feature/AmazingFeature`)
3. 변경사항을 커밋합니다 (`git commit -m 'Add some AmazingFeature'`)
4. 브랜치에 푸시합니다 (`git push origin feature/AmazingFeature`)
5. Pull Request를 생성합니다

## 📝 라이센스

이 프로젝트는 MIT 라이센스 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참조하세요.

## 🔗 관련 링크

- [Claude Code 문서](https://docs.claude.com/claude-code)
- [Arize AX 플랫폼](https://app.arize.com)
- [Arize AX 문서](https://docs.arize.com)
- [Anthropic API 문서](https://docs.anthropic.com)

## 📞 문의

질문이나 제안사항이 있으시면 이슈를 생성해주세요.

## 🙏 감사의 말

- [Anthropic](https://www.anthropic.com/) - Claude Code 제공
- [Arize AI](https://arize.com/) - AX 관찰성 플랫폼 무료 제공
