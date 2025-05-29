# 개요

본 문서는 FastAPI의 Route를 공부하는 글

> router = APIRouter()

api_router = APIRouter()
api_router.include_router(example_base_router, prefix="/example", tags=["example"])
api_router.include_router(example_test_router, prefix="/example", tags=["example"])

FastAPI에서 라우팅을 구조적으로 분리하고 재사용 가능하게 만들기 위한 도구.

> APIRouter는 FastAPI의 서브 라우터로, 여러 API 엔드포인트를 묶어서 메인 앱에 연결할 수 있도록 한다.
