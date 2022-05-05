# Exercise

#### 문제1번) dvd 렌탈 업체의 dvd 대여가 있었던 날짜를 확인해주세요.
```SQL
SELECT DISTINCT date(rental_date)
FROM rental;

-- count 함수로 rental_date 필드의 개수를 파악 결과 중복확인
-- DISTINCT로 중복 제외 출력
```

#### 문제2번) 영화길이가 120분 이상이면서 대여기간이 4일 이상이 가능한 영화제목을 알려주세요.
```SQL
SELECT 	title
FROM	film
WHERE 	length >= 120 AND rental_duration >= 4;

-- WHERE절 활용할 줄 알면 쉽게 풀 수 있음
```

#### 문제3번) 직원의 id 가 2번인 직원의 id, 이름, 성을 알려주세요
```SQL
SELECT	staff_id 
	,first_name
	,last_name 
FROM	staff
WHERE 	staff_id = 2;

-- WHERE절 활용 문제
```

#### 문제4번) 지불 내역 중에서 지불 내역 번호가 17510 에 해당하는 고객의 지출 내역(amount)는 얼마인가요?
```SQL
SELECT	amount
FROM	payment
WHERE 	payment_id = 17510;

-- WHERE절 활용 문제
```

#### 문제5번) 영화 카테고리 중에서 Sci-Fi 카테고리의 카테고리 번호는 몇 번인가요?
```SQL
SELECT	category_id
FROM	category
WHERE 	name = 'Sci-Fi';

-- WHERE절 활용 문제
```

#### 문제6번) film 테이블을 활용하여 rating 등급에 대해서 몇 개의 등급이 있는지 확인해보세요.
```SQL
SELECT	count(DISTINCT rating)
FROM 	film;

-- DISTINCT와 count를 적절히 활용하여 중복 제외한 개수 반환
```

#### 문제7번) 대여 기간이(회수 - 대여일) 10일 이상인 rental 테이블에 대한 모든 정보를 알려주세요. 단, 대여기간은 대여일부터 대여기간으로 포함하여 계산합니다.
```SQL
SELECT	*
FROM 	rental 
WHERE 	date(return_date) - date(rental_date) >= 9;

-- 주어진 날짜 필드끼리 빼려 했으나 주어진 속성이 timestamp인 것 확인
-- timestamp - timestamp = interval -> interval과 단순 integer는 비교 불가

-- 주어진 날짜 필드의 속성을 date로 변환 후 날짜 차이를 계산해야 비교 가능
-- date - date = integer
```
* datetime 함수 관련 [참조링크](https://runebook.dev/ko/docs/postgresql/functions-datetime)

#### 문제8번) 고객의 id가 50,100,150..등 50번의 배수에 해당하는 고객들에 대해서 회원 가입 감사 이벤트를 진행하려고합니다.
#### 고객 아이디가 50번 배수인 아이디와 고객의 이름(성, 이름)과 이메일에 대해서 확인해주세요.
```SQL
SELECT	customer_id
	,first_name 
	,last_name
	,email
FROM	customer
WHERE customer_id % 50 = 0;

-- 50으로 나눈 나머지가 0인 것 활용
```

#### 문제9번) 영화 제목의 길이가 8글자인, 영화 제목 리스트를 나열해주세요.
```SQL
SELECT	title
FROM	film
WHERE 	length(title) = 8;
```

#### 문제10번) city 테이블의 city 갯수는 몇개인가요?
```SQL
SELECT	count(DISTINCT city)
FROM	city;
```

#### 문제11번) 영화배우의 이름(이름+' '+성)에 대해서 대문자로 이름을 보여주세요. 단, 이름이 동일한 사람이 있다면 중복을 제거하고 알려주세요.
```SQL
SELECT DISTINCT concat(first_name, ' ', last_name)
FROM 	actor;

-- 문자열 연결 시 ||와 concat 함수 사용하는 방법 활용
-- text||text -> text
```
* 문자열 연결 함수 관련 [참조링크](https://runebook.dev/ko/docs/postgresql/functions-string#concat)

#### 문제12번) 고객 중에서, active 상태가 0인 즉 현재 사용하지 않고 있는 고객의 수를 알려주세요.
```SQL
SELECT	count(*)
FROM 	customer
WHERE 	active = 0;
```

#### 문제13번) Customer 테이블을 활용하여 store_id = 1 에 매핑된 고객의 수는 몇 명인지 확인해보세요.
```SQL
SELECT	count(*)
FROM 	customer
WHERE 	store_id = 1;
```

#### 문제14번) rental 테이블을 활용하여 고객이 return한 날짜가 2005년6월20일에 해당하는 rental의 갯수가 몇 개인지 확인해보세요.
```SQL
SELECT	count(rental_date)
FROM 	rental
WHERE 	date(return_date) = '2005-06-20';

-- date 타입 헷갈리지 않게 한번 더 공부하기
```
* [date타입 정리 잘 되어있는 블로그](https://m.blog.naver.com/sssang97/222054185948)

#### 문제15번) film 테이블을 활용하여 2006년에 출시가 되고 rating이 'G'등급에 해당하며 대여기간이 3일에 해당하는 것에 대한 film테이블의 모든 컬럼을 알려주세요.
```SQL
SELECT	*
FROM 	film
WHERE 	release_year = '2006'
AND 	rating = 'G'
AND		rental_duration = 3;

-- WHERE절에 여러 조건 활용하는 방법(AND, OR)
```

#### 문제16번) langugage 테이블에 있는 id, name 컬럼을 확인해보세요 .
```SQL
SELECT	language_id
	,name
FROM 	language;
```

#### 문제17번) film 테이블을 활용하여 rental_duration이 7일 이상 대여가 가능한 film에 대해서 film_id, title, description 컬럼을 확인해보세요.
```SQL
SELECT 	film_id
	,title
	,description 
FROM 	film
WHERE 	rental_duration >= 7;
```

### 문제18번) film 테이블을 활용하여 rental_duration 대여가 가능한 일자가 3일 또는 5일에 해당하는 film_id, title, desciption을 확인해주세요.
```SQL
SELECT 	film_id
	,title
	,description 
FROM 	film
WHERE 	rental_duration IN (3, 5);

-- WHERE절에 OR 사용이 여러번 반복될 경우 WHERE ~ IN (a, b, c, ...)형태로 반복을 줄일 수 있음
```

### 문제19번) Actor 테이블을 이용하여, 이름이 Nick이거나 성이 Hunt인 배우의 id와 이름, 성을 확인해주세요.
```SQL
SELECT	actor_id
	,first_name 
	,last_name
FROM 	actor
WHERE 	first_name = 'Nick'
OR 		last_name = 'Hunt';
```

#### 문제20번) Actor 테이블을 이용하여 Actor 테이블의 first_name 컬럼과 last_name 컬럼을 firstname, lastname 으로 컬럼명을 바꿔서 보여주세요.
```SQL
SELECT	actor_id
	,first_name AS firstname
	,last_name AS lastname
FROM 	actor;

-- aliasing 연습
```
