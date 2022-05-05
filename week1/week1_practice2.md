# Exercise

#### 문제1번) film 테이블을 활용하여 film 테이블의 100개의 row만 확인해보세요.
```SQL
SELECT	*
FROM 	film
LIMIT 	100;

-- LIMIT -> 출력할 열의 개수 제한
```

#### 문제2번) actor의 성(last_name)이 Jo로 시작하는 사람의 id값이 가장 낮은 한 사람에 대하여 id, 이름, 성을 알려주세요.
```SQL
SELECT	actor_id
	,last_name 
	,first_name
FROM 	actor
WHERE	last_name LIKE 'Jo%'
ORDER BY actor_id
LIMIT 	1;

-- wildcard를 활용하여 문자열 패턴 파악하는 방법 연습
-- 순서를 정렬하고 원하는 열만큼만 출력하는 연습
```
문자열 패턴 파악 [참조링크](https://www.ikpil.com/1096)

#### 문제3번) film 테이블을 이용하여 film 테이블의 아이디값이 1~10 사이에 있는 모든 컬럼을 확인해주세요.
```SQL
SELECT	*
FROM 	film
WHERE 	film_id BETWEEN 1 AND 10;

-- BETWEEN -> 특정 범위 사이의 값을 구할 때 유용한 함수
-- 시작과 끝 값을 포함하는 특징이 있음
```

#### 문제4번) country 테이블을 이용하여 country이름이 A로 시작하는 country를 확인해주세요.
```SQL
SELECT	country
FROM 	country
WHERE  	country LIKE 'A%';

-- 문자열 패턴 파악 문제 -> wildcard 활용
```

#### 문제5번) country 테이블을 이용하여 country 이름이 s로 끝나는 country 를 확인해주세요.
```SQL
SELECT	country
FROM 	country
WHERE  	country LIKE '%s';
```

#### 문제6번) address 테이블을 이용하여 우편번호(postal_code)값이 77로 시작하는 주소에 대하여 address_id, address, district, postal_code 컬럼을 확인해주세요.
```SQL
SELECT	address_id
	,address
	,district
	,postal_code
FROM 	address
WHERE  	postal_code LIKE '77%';
```

#### 문제7번) address 테이블을 이용하여 우편번호(postal_code)값의 두 번째 글자가 1인 우편번호의 address_id, address, district, postal_code 컬럼을 확인해주세요.
```SQL
SELECT	address_id
	,address
	,district
	,postal_code
FROM 	address
WHERE  	postal_code LIKE '_1%';
```

#### 문제8번) payment 테이블을 이용하여 고객번호가 341에 해당하는 사람이 결제를 2007년 2월 15~16일 사이에 한 모든 결제내역을 확인해주세요.
```SQL
SELECT	*
FROM 	payment
WHERE 	customer_id = 341
AND 	date(payment_date) BETWEEN '2007-2-15' AND '2007-2-16';
```

#### 문제9번) payment 테이블을 이용하여 고객번호가 355에 해당하는 사람의 결제 금액이 1~3원 사이에 해당하는 모든 결제 내역을 확인해주세요.
```SQL
SELECT	*
FROM 	payment
WHERE 	customer_id = 355
AND 	amount BETWEEN 1 AND 3;
```

#### 문제10번) customer 테이블을 이용하여 고객의 이름이 Maria, Lisa, Mike에 해당하는 사람의 id, 이름, 성을 확인해주세요.
```SQL
SELECT	customer_id
	,last_name 
	,first_name 
FROM 	customer
WHERE 	name IN ('Maria', 'Lisa', 'Mike');
```

#### 문제11번) film 테이블을 이용하여 film의 길이가 100\~120에 해당하거나 또는 rental기간이 3\~5일에 해당하는 film의 모든 정보를 확인해주세요.
```SQL
SELECT	*
FROM 	film
WHERE 	length BETWEEN 100 AND 120
OR      rental_duration  BETWEEN 3 AND 5;
```

#### 문제12번) address 테이블을 이용하여 postal_code 값이 공백('')이거나 35200, 17886에 해당하는 address에 모든 정보를 확인해주세요.
```SQL
SELECT	*
FROM 	address
WHERE 	postal_code IN ('', '35200', '17886');

--  입력된 필드의 데이터타입 제대로 확인하고 쿼리 짜야 에러없음!
```

#### 문제13번) address 테이블을 이용하여 address의 상세주소(=address2) 값이 존재하지 않는 모든 데이터를 확인하여 주세요.
```SQL
SELECT	*
FROM 	address
WHERE 	address2 IS NULL;
```

#### 문제14번) staff 테이블을 이용하여 staff의 picture 사진의 값이 있는직원의 id, 이름, 성을 확인해주세요.
#### 단, 이름과 성을 하나의 컬럼으로 이름, 성의 형태로 새로운 컬럼 name 컬럼으로 도출해주세요.
```SQL
SELECT	staff_id
	,concat(last_name, ', ', first_name) AS name
FROM 	staff
WHERE   picture IS NOT NULL;
```

#### 문제15번) rental 테이블을 이용하여 대여는 했으나 아직 반납 기록이 없는 대여 건의 모든 정보를 확인해주세요.
```SQL
SELECT	*
FROM 	rental
WHERE 	rental_date IS NOT NULL
AND 	return_date IS NULL;
```

#### 문제16번) address 테이블을 이용하여 postal_code 값이 빈 값(NULL)이거나 35200, 17886에 해당하는 address의 모든 정보를 확인해주세요.
```SQL
SELECT	*
FROM 	address
WHERE 	postal_code IN (NULL, '35200', '17886');
```

#### 문제17번) 고객의 성에 John이라는 단어가 들어가는 고객의 이름과 성을 모두 찾아주세요.
```SQL
SELECT	last_name
	,first_name 
FROM 	customer
WHERE 	first_name LIKE '%John%'
```
문자열 패턴 파악 [참조링크2](https://dog-developers.tistory.com/136)

#### 문제18번) address 테이블에서 address2 값이 null 값인 row 전체를 확인해주세요.
```SQL
SELECT	*
FROM 	address
WHERE  	address2 IS NULL;
```
