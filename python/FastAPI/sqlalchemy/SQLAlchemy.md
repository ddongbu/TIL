# 개요

SQLAlchemy는 Python에서 데이터베이스와의 연결 및 ORM 등을 활용할 수 있도록 해주는 라이브러리 입니다.
가령 특정 쿼리를 코드에서 실행할 수 있고, ORM 객체를 통해 데이터베이스에서의 일련의 작업들을 수행할 수 있습니다.

# 설치

```bash
pip install sqlalchemy
버전 확인
>>> import sqlalchemy
>>> sqlalchemy.__version__

```

#제공되는 것
SQLAlchemy는 다음처럼 2가지로 제공됩니다.

- Core
  데이터베이스 도구 키트로, SQLAlchemy의 기본 아키텍처입니다.
  데이터베이스에 대한 연결을 관리하고, 데이터베이스 쿼리 및 결과와 상호 작용하고, SQL 문을 프로그래밍 방식으로 구성하기위한 도구를 제공합니다.
- ORM
  Core를 기반으로 구축되어 선택적 ORM 기능을 제공 합니다.
  기본적으로 Core에 대해 먼저 이해한 후, ORM을 사용하는게 좋습니다.
  튜토리얼 역시 Core부터 설명합니다.
