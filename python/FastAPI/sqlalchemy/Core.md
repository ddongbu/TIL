# Core

> SQLAlchemy에서 가장 자주 쓰이는 Select에 대해 공부하며 자주 사용한 부분만 작성했다.

## select() 를 통한 SQL 표현식 구성

select() 생성자는 insert() 생성자와 같은 방식으로 쿼리문을 만들 수 있다.

```python
stmt = select(user_table).where(user_table.c.name == "danny")
print(stmt)

SELECT user_table.id, user_table.name, ...
FROM user_table
WHERE user_table.name = :name_1
```

## select

ORM 엔터티 및 열 조회
User 객체같은 ORM 엔터티나, User.name과 같이 컬럼이 매핑된 속성을 사용할 수 있다.

```python
select(User)
SELECT user.id, user.name, user.fullname
FROM user
```

## ORDER BY, GROUP BY, HAVING

- ORDER BY절은 SELECT절에서 조회한 행들의 순서를 설정할 수 있습니다.
- GROUP BY절은 그룹 함수로 조회된 행들을 특정한 컬럼을 기준으로 그룹을 만듭니다.
- HAVING은 GROUP BY절을 통해 생성된 그룹에 대해 조건을 겁니다.

## order_by

Select.order_by()를 이용해 ORDER BY기능을 구현할 수 있으며 이때 위치 인자 값으로 Column객체나 이와 비슷한 객체들을 받을 수 있다.

asc(), desc() 제한자를 통해 구현 가능하며 asc는 생략가능

## 별칭 사용하기

여러개의 테이블을 JOIN을 이요해 조회 할 경우 쿼리문에서 테이블 이름을 여러번 적어줘야 하는 경우가 많다. 이러한 문제를 테이블 명이나 서브 쿼리에 별칭을 지어 반복되는 부분을 줄일 수 있다.
한편 SQLAlchemy에서는 이러한 별칭들은 Core의 alias() 함수를 이용해 구현 할 수 있다.
Table 객체 네임스페이스 안에 Column 객체가 있어 Table.c로 컬럼명에 접근 할 수 있다.

## 서브쿼리

Subquery 개체를 Select.subquery() 사용하여 서브 쿼리를 나타낸다.

```python
subq = (
    select(
        orders.c.user_id,
        func.count().label("order_count")
    )
    .group_by(orders.c.user_id)
    .subquery()
)

SELECT user_id, COUNT(*) AS order_count
FROM orders
GROUP BY user_id
```

```python
stmt = (
    select(users.c.name, subq.c.order_count)
    .join(subq, users.c.id == subq.c.user_id)
    .order_by(subq.c.order_count.desc())
)

SELECT users.name, subq.order_count
FROM users
JOIN (
    SELECT user_id, COUNT(*) AS order_count
    FROM orders
    GROUP BY user_id
) AS subq ON users.id = subq.user_id
ORDER BY subq.order_count DESC
```

## 윈도우 함수

func 네임스페이스에 의해 생성된 모든 SQL 함수 중 하나로 OVER 구문을 구현하는 FunctionElement.over() 메서드가 있다.

행의 개수를 세는 row_number() 함수가 있다!!!
각 행을 사용자 이름대로 그룹을 나누고 그 안에서 이메일 주소에 번호를 매길 수 있다.

```py
stmt = select(
    func.row_number().over(partition_by=user_table.c.name),
    user_table.c.name,
    address_table.c.email_address
).select_from(user_table).join(address_table)

[(1, 'sandy', 'sandy@sqlalchemy.org'),
 (2, 'sandy', 'sandy@squirrelpower.org'),
 (1, 'spongebob', 'spongebob@sqlalchemy.org')]

 func.count().over(order_by=user_table.c.name)

 ORDER BY 절도 사용가능

```
