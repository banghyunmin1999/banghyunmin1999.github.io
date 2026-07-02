---
title: FastAPI 로그를 loguru로 통합하기
date: 2025-12-02 23:50:00 +0900
categories: [학습, Python]
tags: [python, fastapi, loguru, logging]
---

## fastapi 설치

```python
!pip install fastapi uvicorn jinja2 loguru
```

설치된 fastapi 버전 확인

```python
!pip show fastapi uvicorn jinja2 loguru
```

jupyter lab 에서 fastapi 사용해보기

```python
from fastapi import FastAPI , Request
import uvicorn
# FastAPI: 웹 API 서버를 만들기 위한 메인 프레임워크 클래스
# Request: 클라이언트의 HTTP 요청 정보를 담는 객체
from fastapi.templating import Jinja2Templates
# Jinja2Templates: HTML 템플릿 파일을 렌더링하여 동적 웹페이지를 만드는 엔진
```

```python
# fastapi 로그 출력을 loguru 출력으로 설정
from module_01_fastapi_log_to_loguru import setup_logging, get_logger

# 로깅 설정 적용
setup_logging()
logger = get_logger(__name__)
logger.info("Logging 설정이 완료되었습니다.")
```

```text
2025-12-02 23:50:52.398 | INFO     | module_01_fastapi_log_to_loguru:setup_logging:77 - Logging 설정이 완료되었습니다.
2025-12-02 23:50:52.399 | INFO     | __main__:<module>:7 - Logging 설정이 완료되었습니다.
```

```python
# Jupyter Notebook에서 fastapi 테스트 하기 위한 라이브러리
# !pip install nest-asyncio
import nest_asyncio
nest_asyncio.apply()
# 실행 하려면 셀 안에 아래 처럼 실행
# config = uvicorn.Config(app, host="127.0.0.1", port=8000, log_config=None)
# server = uvicorn.Server(config=config)
# await server.serve()
```

fastapi 인스턴스 생성

```python
app = FastAPI()
```

fastapi 루트 경로 read 요청 받고 응답 보내기

```python
@app.get("/")
def read_root():
    logger.info("read_root 요청")
    return {"message" : "루트 경로 응답 보냄"}
```

```python
# Uvicorn 서버 설정 (Loguru 설정은 setup_logging()에서 완료됨)
config = uvicorn.Config(
    app,
    host="127.0.0.1",
    port=8000,
    log_config=None,  # Loguru 사용으로 기본 로깅 비활성화
    log_level="debug",
)

logger.info("Starting Uvicorn server...")
server = uvicorn.Server(config=config)

await server.serve()
```
