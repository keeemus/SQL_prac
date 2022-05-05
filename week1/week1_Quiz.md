# Quiz

#### 문제1번) 각 제품 가격을 5% 줄이려면 어떻게 해야 할까요?
```SQL
SELECT 	*
	,(retailprice - retailprice * 0.05) AS new_retailprice
FROM 	products;
```

#### 문제2번) 고객이 주문한 목록을 주문일자로 내림차순 정렬해서 보여주세요.
```SQL
SELECT 	*
FROM 	orders
ORDER BY orderdate DESC;
```

#### 문제3번) employees 테이블을 이용하여 705 아이디를 가진 직원의 이름, 성과 해당 직원의 태어난 해를 확인해주세요.
```SQL
SELECT 	emplastname
	,empfirstname
	,EXTRACT('YEAR' FROM empbirthdate)
FROM 	employees
WHERE 	employeeid = 705;

-- EXTRACT로 원하는 날짜 추출하는 방법 공부
```
[날짜 추출 참조링크](https://sas-study.tistory.com/387)

#### 문제4번) customers 테이블을 이용하여 고객의 이름과 성을 하나의 컬럼으로 전체 이름을 보여주세요(이름, 성의 형태로 full_name 이라는 이름으로 보여주세요.)
```SQL
SELECT 	concat(custlastname, ', ', custfirstname) AS full_name
FROM 	customers;
```

#### 문제5번) orders 테이블을 활용하여 고객번호가 1001 에 해당하는 사람이 employeeid 가 707인 직원으로부터  산 주문의 id 와 주문 날짜를 알려주세요.
#### 주문일자 빠른순으로 정렬하여 보여주세요.
```SQL
SELECT 	ordernumber
	,orderdate
FROM	orders
WHERE 	customerid = 1001
AND 	employeeid = 707
ORDER BY orderdate;
```

#### 문제6번) vendors 테이블을 이용하여 벤더가 위치한 state 주가 어떻게되는지 확인해보세요. 중복된 주가 있다면 제거 후에 알려주세요.
```SQL
SELECT 	vendname
	,DISTINCT vendstate
FROM	vendors;
```

#### 문제7번) 주문일자가 2017-09-02~09-03 사이에 해당하는 주문번호를 알려주세요.
```SQL
SELECT 	ordernumber
FROM 	orders
WHERE 	orderdate BETWEEN '2017-09-02' AND '2017-09-03';
```

#### 문제8번) products 테이블을 활용하여 productdescription에 상품 상세 설명 값이 없는 상품 데이터를 모두 알려주세요.
```SQL
SELECT 	*
FROM 	products
WHERE 	productdescription IS NULL;
```

#### 문제9번) vendors 테이블을 이용하여 vendor의 State 지역이 NY 또는 WA인 업체의 이름을 알려주세요.
```SQL
SELECT 	vendname
FROM 	vendors
WHERE 	vendstate IN ('NY', 'WA');
```

#### 문제10번) customers 테이블을 이용하여 고객의 id별로 custstate 지역 중 WA 지역에 사는 사람과 WA가 아닌 지역에 사는 사람을 구분해서 보여주세요.
* customerid와 newstate_flag 컬럼으로 구성해주세요.
* newstate_flag 컬럼은 WA와 OTHERS로 노출해주시면 됩니다.
```SQL
SELECT 	customerid,
CASE
	WHEN	custstate = 'WA' THEN 'WA'
	ELSE 	'OTHERS'
END AS 	newstaet_flag
FROM 	customers;

-- 설마 case문 활용하나 싶었는데 진짜 활용하는 거였다,,
-- 처음에 어버버하고 조금 해맸는데 그래도 잘 풀었음 !
```
[case문 참조 링크(PostgreSQL 공식문서)](https://runebook.dev/ko/docs/postgresql/functions-conditional#FUNCTIONS-CASE)
