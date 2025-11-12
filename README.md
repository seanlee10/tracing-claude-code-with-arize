# Tracing Claude Code with Arize

Claude Code의 실행을 Arize 플랫폼으로 추적하고 모니터링하는 프로젝트입니다.

## 📖 개요

이 프로젝트는 Anthropic의 Claude Code CLI 도구의 동작을 Arize Phoenix를 사용하여 추적하고 관찰할 수 있도록 합니다. Claude Code의 API 호출, 응답 시간, 토큰 사용량, 에러 등을 실시간으로 모니터링하고 분석할 수 있습니다.

## ✨ 주요 기능

- **실시간 추적**: Claude Code의 모든 API 호출을 실시간으로 추적
- **성능 모니터링**: 응답 시간, 토큰 사용량, 비용 분석
- **에러 추적**: 실패한 요청과 에러 로그 수집
- **시각화**: Arize Phoenix 대시보드를 통한 직관적인 데이터 시각화
- **디버깅**: 상세한 트레이스 정보로 문제 진단 및 해결

## 🚀 시작하기

### 사전 요구사항

- Python 3.8 이상
- Claude Code CLI
- Arize Phoenix 계정 (선택사항)

### 설치

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

# Arize Phoenix 설정 (선택사항)
PHOENIX_COLLECTOR_ENDPOINT=your_endpoint_here
PHOENIX_API_KEY=your_phoenix_api_key_here
```

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

### Arize Phoenix와 연동

```python
from claude_tracer import ClaudeTracer
from phoenix.trace import PhoenixTracer

# Phoenix 트레이서 초기화
phoenix_tracer = PhoenixTracer()

# Claude 트레이서 초기화
tracer = ClaudeTracer(backend=phoenix_tracer)

# 추적 시작
tracer.start()

# Claude Code 작업 수행
# ...

# Phoenix 대시보드에서 결과 확인
```

### 대시보드 실행

```bash
# Phoenix 대시보드 시작
python -m phoenix.server

# 브라우저에서 http://localhost:6006 접속
```

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
- [Arize Phoenix 문서](https://docs.arize.com/phoenix)
- [Anthropic API 문서](https://docs.anthropic.com)

## 📞 문의

질문이나 제안사항이 있으시면 이슈를 생성해주세요.

## 🙏 감사의 말

- [Anthropic](https://www.anthropic.com/) - Claude Code 제공
- [Arize AI](https://arize.com/) - Phoenix 관찰성 플랫폼 제공
