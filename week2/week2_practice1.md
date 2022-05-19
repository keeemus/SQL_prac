# Exercise

#### 문제1번) 고객의 기본 정보인 고객 id, 이름, 성, 이메일과 함께 고객의 주소 address, district, postal_code, phone 번호를 함께 보여주세요.
```SQL
SELECT 	c.customer_id
	,c.last_name  
	,c.first_name 
	,c.email
	,a.address 
	,a.district
	,a.postal_code
	,a.phone
FROM 	customer c
	INNER JOIN address a
	        ON c.address_id = a.address_id;

-- 기본적인 join 문제
-- 기준이 되는 필드를 잘 선택해서 접근하면 어려울 게 없음 ! 
```

#### 문제2번) 고객의 기본 정보인, 고객 id, 이름, 성, 이메일과 함께 고객의 주소 address, district, postal_code, phone, city를 함께 알려주세요.
```SQL
SELECT 	c1.customer_id
	,c1.last_name  
	,c1.first_name 
	,c1.email
	,a.address 
	,a.district
	,a.postal_code
	,a.phone
	,c2.city
FROM    customer c1
	INNER JOIN address a
		ON c1.address_id = a.address_id
	INNER JOIN city c2
		ON c2.city_id = a.city_id;
```

#### 문제3번) Lima City에 사는 고객의 이름과, 성, 이메일, phonenumber에 대해서 알려주세요.
```SQL
SELECT 	c1.last_name  
	,c1.first_name 
	,c1.email
	,a.phone
	,c2.city
FROM    customer c1
	INNER JOIN address a
		ON c1.address_id = a.address_id
	INNER JOIN city c2
		ON c2.city_id = a.city_id
WHERE   c2.city = 'Lima';
```

#### 문제4번) rental 정보에 추가로 고객의 이름과 직원의 이름을 함께 보여주세요.
- 고객의 이름, 직원 이름은 이름과 성을 fullname 컬럼으로만들어서 직원이름/고객이름 2개의 컬럼으로 확인해주세요.
```SQL
SELECT 	r.*
	,concat(c.last_name, ' ', c.first_name) AS 고객이름
	,concat(s.last_name, ' ', s.first_name) AS 직원이름
FROM 	rental r
	INNER JOIN customer c
		ON c.customer_id = r.customer_id
	INNER JOIN staff s
		ON s.staff_id = r.staff_id;
```

#### 문제5번) seth.hannon@sakilacustomer.org 이메일 주소를 가진 고객의 주소 address, address2, postal_code, phone, city주소를 알려주세요.
```SQL
SELECT 	a.address
	,a.address2 
	,a.postal_code
	,a.phone 
	,c2.city
FROM 	address a
	INNER JOIN customer c1
		ON c1.address_id = a.address_id
	INNER JOIN city c2
		ON c2.city_id = a.city_id
WHERE   c1.email = 'seth.hannon@sakilacustomer.org';

-- join 테이블에 where로 조건 걸어서 정보 조회하는 연습
```

#### 문제6번) Jon Stephens 직원을 통해 dvd대여를 한 payment기록 정보를 확인하려고 합니다.
- payment_id, 고객이름과 성, rental_id, amount, staff이름과 성을 알려주세요.
```SQL
SELECT 	p.payment_id
	,c.last_name
	,c.first_name 
	,p.rental_id
	,p.amount
	,s.last_name
	,s.first_name
FROM 	payment p
	INNER JOIN customer c
		ON c.customer_id = p.customer_id
	INNER JOIN staff s 
		ON s.staff_id = p.staff_id
WHERE   s.first_name = 'Jon' AND s.last_name = 'Stephens';
```

#### 문제7번) 배우가 출연하지 않는 영화의 film_id, title, release_year, rental_rate, length를 알려주세요.
```SQL
SELECT 	f1.film_id
	,f1.title 
	,f1.release_year
	,f1.rental_rate 
	,f1.length
FROM 	film f1
	INNER JOIN film_actor f2
		ON f1.film_id = f2.film_id
	INNER JOIN actor a 
		ON f2.actor_id = a.actor_id
```

#### 문제8번) store 상점 id별 주소(address, address2, distict)와 해당 상점이 위치한 city 주소를 알려주세요.
```SQL
SELECT 	s.store_id 
	,a.address
	,a.address2 
	,a.district 
	,c.city
FROM 	store s
	INNER JOIN address a
		ON s.address_id = a.address_id
	INNER JOIN city c 
		ON a.city_id = c.city_id;
```

#### 문제9번) 고객의 id 별로 고객의 이름(first_name, last_name), 이메일, 고객의 주소(address, district), phone번호, city, country를 알려주세요.
```SQL
SELECT 	c1.customer_id 
	,c1.first_name
	,c1.last_name 
	,c1.email 
	,a.address
	,a.district
	,a.phone
	,c2.city 
	,c3.country
FROM 	customer c1
	INNER JOIN address a
		ON c1.address_id = a.address_id
	INNER JOIN city c2
		ON a.city_id = c2.city_id
	INNER JOIN country c3
		ON c2.country_id = c3.country_id;
```

#### 문제10번) country가 china가 아닌 지역에 사는 고객의 이름(first_name, last_name)과 email, phonenumber, country, city를 알려주세요
```SQL
SELECT 	c1.first_name
	,c1.last_name 
	,c1.email 
	,a.phone
	,c3.country
	,c2.city 
FROM 	customer c1
	INNER JOIN address a
		ON c1.address_id = a.address_id
	INNER JOIN city c2
		ON a.city_id = c2.city_id
	INNER JOIN country c3
		ON c2.country_id = c3.country_id
WHERE   c3.country != 'China';
```

#### 문제11번) Horror 카테고리 장르에 해당하는 영화의 이름과 description에 대해서 알려주세요
```SQL
SELECT 	f.title
	,f.description 
FROM 	film f
	INNER JOIN film_category f2
		ON f.film_id = f2.film_id
	INNER JOIN category c
		ON c.category_id = f2.category_id
WHERE   c.name = 'Horror';
```

#### 문제12번) Music 장르이면서, 영화길이가 60~180분 사이에 해당하는 영화의 title, description, length 를 알려주세요.
- 영화 길이가 짧은 순으로 정렬해서 알려주세요.
```SQL
SELECT 	f.title
	,f.description
	,f.length
FROM 	film f
	INNER JOIN film_category f2
		ON f.film_id = f2.film_id
	INNER JOIN category c
		ON c.category_id = f2.category_id
WHERE 	c.name = 'Music' 
	AND f.length BETWEEN 60 AND 180
ORDER BY f.length;
```

#### 문제13번) actor 테이블을 이용하여 배우의 ID, 이름, 성 컬럼에 추가로 'Angels Life' 영화에 나온 영화 배우 여부를 Y, N으로 컬럼을 추가 표기해주세요.</br>해당 컬럼은 angelslife_flag로 만들어주세요.
```SQL
SELECT a.first_name
       ,a.last_name
       ,CASE
        WHEN a.actor_id IN (SELECT f2.actor_id
                             FROM   film f1
                                    INNER JOIN film_actor f2
                                            ON f1.film_id = f2.film_id
                             WHERE  f1.title = 'Angels Life') THEN 'Y'
        ELSE 'N'
       END AS angelslife_flag
FROM   actor a; 

-- 서브쿼리 이용해본 첫 문제 ! 
-- 아이디어 떠올리기까지 시간이 조금 걸렸음
-- 서브쿼리 계속 연습해보면 금방 적응될 것 같다.
```

#### 문제14번) 대여일자가 2005-06-01~14일에 해당하는 주문 중에서 직원의 이름(이름 성) = 'Mike Hillyer'이거나 </br>고객의 이름이 (이름 성) = 'Gloria Cook'에 해당하는 rental의 모든 정보를 알려주세요.
- 추가로 직원이름과 고객이름에 대해서도 fullname으로 구성해서 알려주세요.
```SQL
SELECT	r.*
	,concat(c.first_name, ' ', c.last_name)
	,concat(s.first_name, ' ', s.last_name)
FROM 	rental r
	INNER JOIN staff s 
		ON r.staff_id = s.staff_id
	INNER JOIN customer c
		ON r.customer_id = c.customer_id
WHERE 	r.rental_date::date BETWEEN '2005-06-01' AND '2005-06-14'
	AND concat(s.first_name, ' ', s.last_name) = 'Mike Hillyer'
	OR  concat(c.first_name, ' ', c.last_name) = 'Gloria Cook';
 
-- rental_date 필드를 date 타입으로 변환해야하는 거 주의하기
```

#### 문제15번) 대여일자가 2005-06-01~14일에 해당하는 주문 중에서 직원의 이름(이름 성) = 'Mike Hillyer'인 직원에게 구매하지 않은 </br>rental의 모든 정보를 알려주세요.
- 추가로 직원이름과 고객이름에 대해서도 fullname으로 구성해서 알려주세요.
```SQL
SELECT	r.*
	,concat(c.first_name, ' ', c.last_name)
	,concat(s.first_name, ' ', s.last_name)
FROM 	rental r
	INNER JOIN staff s 
		ON r.staff_id = s.staff_id
	INNER JOIN customer c
		ON r.customer_id = c.customer_id
WHERE 	r.rental_date::date BETWEEN '2005-06-01' AND '2005-06-14'
	AND concat(s.first_name, ' ', s.last_name) = 'Mike Hillyer';
```
