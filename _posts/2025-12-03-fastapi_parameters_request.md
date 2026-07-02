---
title: FastAPI 파라미터 실습 (Path·Query·Request Body)
date: 2025-12-03 21:24:00 +0900
categories: [Python, FastAPI]
tags: [python, fastapi, pydantic]
---

이곳에서 하나씩 직접 코드를 작성하며 익혀봅시다.

## 미션 0: 기본 설정 및 라이브러리 임포트

FastAPI를 실행하기 위한 기본적인 준비 단계입니다.

**목표**:
1. 필요한 라이브러리를 임포트하세요 (`FastAPI`, `uvicorn`, `nest_asyncio`, `BaseModel` 등).
2. `src` 폴더에 있는 우리의 커스텀 로깅 모듈(`module_01_fastapi_log_to_loguru`)을 불러오세요.
3. 로깅을 설정(`setup_logging()`)하고, `app`이라는 이름으로 FastAPI 인스턴스를 생성하세요.

```python
# 여기에 미션 0 코드를 작성해보세요
import sys
from fastapi import FastAPI
import uvicorn
import nest_asyncio
from loguru import logger
sys.path.append("src")

from module_01_fastapi_log_to_loguru import setup_logging

nest_asyncio.apply()
setup_logging()

app = FastAPI()
```

```text
2025-12-03 21:24:19.473 | INFO     | module_01_fastapi_log_to_loguru:setup_logging:77 - Logging 설정이 완료되었습니다.
```

## 미션 1: Pydantic 모델(BaseModel) 정의하기

데이터를 주고받을 때 사용할 데이터의 형식을 정의합니다.

**목표**:
1. `pydantic`에서 `BaseModel`을 상속받는 `Item` 클래스를 만드세요.
2. `Item` 클래스는 `name` (문자열)과 `price` (숫자) 필드를 가져야 합니다.

> **💡 힌트**
> - 클래스 상속 문법: `class 클래스이름(부모클래스):`
> - 타입 힌트 문법: `변수명: 타입` (예: `age: int`)

```python
# 여기에 미션 1 코드를 작성해보세요
from pydantic import BaseModel
class Item(BaseModel):
    name: str
    price: int = 5000
```

## 미션 2: Path Parameter와 모델 사용하기

URL 경로로 아이디를 받고, 응답으로는 방금 만든 `Item` 모델을 돌려주는 API를 만들어봅시다.

**목표**:
1. `GET` 메서드로 `/items/{item_id}` 경로를 만드세요.
2. `item_id`를 함수의 인자로 받으세요.
3. 함수 내부에서 `Item` 객체를 생성해서 리턴하세요. (이름과 가격은 자유롭게 넣으세요)

> **💡 힌트**
> - 데코레이터: `@app.get("...")`
> - 경로 변수: 중괄호 `{}` 사용
> - 리턴 값: `Item(...)` 객체를 바로 리턴하면 FastAPI가 알아서 JSON으로 바꿔줍니다.

```python
# 여기에 미션 2 코드를 작성해보세요
@app.get("/items/{item_id}")
def read_item_id(item_id):
    item = Item(name="item_" + str(item_id))
    return item
```

## 미션 3: Query Parameter (쿼리 매개변수)

URL 경로에는 없지만 `?key=value` 형태로 데이터를 받는 방법입니다.
검색 필터나 페이지네이션(page, limit) 등에 주로 쓰입니다.

**작동 원리**:
경로(`{}`)에 없는 변수를 함수 인자로 선언하면, FastAPI는 자동으로 **쿼리 매개변수**로 인식합니다.

**목표**:
1. `GET` 메서드로 `/users/` 경로를 만드세요.
2. `skip` (정수, 기본값 0)과 `limit` (정수, 기본값 10)을 함수 인자로 받으세요.
3. 받은 `skip`과 `limit`을 포함한 딕셔너리를 리턴하세요.

> **💡 힌트**
> - 경로에는 `{}`를 쓰지 않습니다: `@app.get("/users/")`
> - 함수 인자에 기본값을 주면 선택적(Optional) 파라미터가 됩니다: `def read_users(skip: int = 0, ...):`
> - 테스트 URL 예시: `http://127.0.0.1:8000/users/?skip=20&limit=5`

```python
# 여기에 미션 3 코드를 작성해보세요
@app.get("/user/")
def read_user(skip: int = 0,limit: int = 10):
    return {"skip": skip, "limit": limit}
```

## 미션 4: Request Body (요청 바디)

클라이언트가 JSON 데이터를 보내면, 그걸 받아서 처리하는 방법입니다.
주로 데이터를 생성(`POST`)하거나 수정(`PUT`)할 때 사용합니다.

**작동 원리**:
함수 인자의 타입으로 **Pydantic 모델**(`Item` 등)을 지정하면, FastAPI는 "아, 요청 바디(Body)에 JSON으로 데이터가 들어오겠구나!" 하고 인식합니다.

**목표**:
1. `POST` 메서드로 `/items/` 경로를 만드세요. (주의: `GET`이 아니라 `POST`입니다!)
2. 함수 인자로 `item: Item`을 받으세요. (미션 1에서 만든 `Item` 클래스 사용)
3. 받은 `item` 객체를 그대로 리턴하세요.

> **💡 힌트**
> - 데코레이터: `@app.post("/items/")`
> - 함수 인자: `def create_item(item: Item):`
> - **테스트 방법**: 브라우저 주소창으로는 POST 요청을 보낼 수 없습니다.
>   - `http://127.0.0.1:8000/docs` (Swagger UI)에 접속해서 테스트해야 합니다.
>   - 'Try it out' 버튼을 누르고 JSON 데이터를 입력해서 'Execute' 해보세요.

```python
# 여기에 미션 4 코드를 작성해보세요
@app.post("/items/")
def create_item(item: Item):
    logger.info(item)
    return item
```

```python
# 서버 실행 코드 (모든 미션 코드를 작성한 후 실행하세요)
if __name__ == "__main__":
    config = uvicorn.Config(app, host="127.0.0.1", port=8000, log_config=None)
    server = uvicorn.Server(config=config)
    await server.serve()
```
