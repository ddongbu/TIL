# 연결 설정

### 데이터베이스와 연결

🔍 코드 흐름 정리

1. DatabaseSessionManager 클래스

```python
self.engines = {
    "DB": create_async_engine(str(settings.SYSTEM_DB), ...)
}
```

"DB" 라는 이름으로 async 엔진을 생성,
위 엔진에 대해 각각 async_sessionmaker를 생성 → 나중에 세션을 만들 수 있는 "공장"이 됩니다.

```python
async def get_db_session(db_name: str):
    async with database.async_session_factory[db_name]() as session:
        try:
            yield session
            await session.commit()
        except Exception as e:
            await session.rollback()
            raise e
        finally:
            await session.close()


database = DatabaseSessionManager()
get_db_session = partial(get_db_session, "DB")
```

호출할 때마다 async_sessionmaker을 사용해서 새로운 AsyncSession 인스턴스 생성, 구문을 통해 세션의 생성 -> 사용 -> 커밋 또는 롤백 -> 종료까지 완전히 관리

필자는 Service -> Repository 패턴을 위해 Inject에서 의존성 설정

```python
def get_example_feat_service(request: Request, db: AsyncSession = Depends(get_system_db_session)):
    feat_example_repo = FeatExampleRepository(db=db)
    feat_service = FeatExampleService(repo=feat_example_repo, request=request)
    return feat_service
```
