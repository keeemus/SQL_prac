# Exercise

#### 문제1번) store 별로 staff는 몇 명이 있는지 확인해주세요.
```SQL
SELECT  store_id
        ,count(*)
FROM    staff
GROUP BY store_id;
```

#### 문제2번) 영화등급(rating) 별로 몇 개 영화 film을 가지고 있는지 확인해주세요.
```SQL
SELECT	rating
        ,count(*)
FROM    film
GROUP BY rating;
```

#### 문제3번) 출현한 영화배우(actor)가 10명 초과한 영화명은 무엇인가요?
```SQL
SELECT	film.title
FROM	(SELECT	film_actor.film_id
	        ,count(*)
         FROM	film_actor
         GROUP BY film_actor.film_id
         HAVING	count(*) > 10) f
        INNER JOIN film
                ON film.film_id = f.film_id;

-- 이제까지 푼 문제 중 가장 고민 많이 한 문제
-- 서브쿼리 잘 활용할줄 알면 쉽게 해결할 수 있는 문제이고
-- 서브쿼리 aliasing 필수(인라인 뷰) !
```

#### 문제4번) 영화 배우(actor)들이 출연한 영화는 각각 몇 편인가요?
- 영화 배우의 이름, 성과 함께 출연 영화 수를 알려주세요.
```SQL
SELECT actor.first_name
       ,actor.last_name
       ,f.cnt
FROM   (SELECT film_actor.actor_id,
               count(*) AS cnt
        FROM   film_actor
        GROUP BY film_actor.actor_id) f
       INNER JOIN actor
               ON f.actor_id = actor.actor_id;
               
-- 어렵다 ! ..
```
[서브쿼리 참고 블로그](https://jeongkyun-it.tistory.com/38)

#### 문제5번) 국가(country)별 고객(customer)는 몇명인가요?
```SQL
SELECT count(*)
FROM   customer c
       INNER JOIN address a
               ON c.address_id = a.address_id
       INNER JOIN city c2
               ON a.city_id = c2.city_id
       INNER JOIN country c3
               ON c2.country_id = c3.country_id
GROUP BY c3.country; 
```

#### 문제6번) 영화 재고(inventory) 수량이 3개 이상인 영화(film)는?
- store는 상관 없이 확인해주세요.
```SQL
SELECT f.title
       ,count(i.film_id)
FROM   film f
       INNER JOIN inventory i
               ON f.film_id = i.film_id
GROUP BY f.title
HAVING count(i.film_id) > 3
ORDER BY f.title; 

--서브쿼리 사용
SELECT f.title
       ,i.cnt
FROM   (SELECT film_id,
               count(*) AS cnt
        FROM   inventory i
        GROUP BY film_id
        HAVING count(*) > 3) AS i
       INNER JOIN film f
               ON f.film_id = i.film_id
ORDER BY f.title; 
```

#### 문제7번) dvd 대여를 제일 많이한 고객 이름은?
```SQL
SELECT c.first_name
       ,c.last_name
FROM   customer c
       INNER JOIN (SELECT customer_id,
                          count(*) AS cnt
                   FROM   payment p
                   GROUP BY customer_id
                   ORDER BY count(*) DESC
                   LIMIT  1) p
               ON c.customer_id = p.customer_id;

-- 서브쿼리 없이 조인만 사용해서 해결하려 했으나 group by에서 제한
```

#### 문제8번) rental 테이블을 기준으로 2005년 5월26일에 대여를 기록한 고객 중 하루에 2번 이상 대여를 한 고객의 ID 값을 확인해주세요.
```SQL
SELECT r.customer_id
       ,count(*)
FROM   rental r
WHERE  date(r.rental_date) = '2005-05-26'
GROUP BY r.customer_id
HAVING count(*) >= 2; 
```

#### 문제9번) film_actor 테이블을 기준으로 출현한 영화의 수가 많은 5명의 actor_id와 출현한 영화 수를 알려주세요.
```SQL
SELECT fa.actor_id
       ,count(*)
FROM   film_actor fa
GROUP BY fa.actor_id
ORDER BY count(*) DESC
LIMIT  5; 
```

#### 문제10번) payment 테이블을 기준으로 결제일자가 2007년 2월 15일에 해당하는 주문 중에서 하루에 2건 이상 주문한 고객의 총 결제 금액이 10달러 이상인 고객에 대해서 알려주세요.
- 고객의 id, 주문건수, 총 결제 금액까지 알려주세요.
```SQL
SELECT p.customer_id
       ,count(*)
       ,sum(p.amount)
FROM   payment p
WHERE  date(p.payment_date) = '2007-02-15'
GROUP BY p.customer_id
HAVING count(*) >= 2
       AND sum(p.amount) >= 10 
```

#### 문제11번) 사용되는 언어별 영화 수는?
- 고객의 id, 주문건수, 총 결제 금액까지 알려주세요.
```SQL
SELECT l.name,
       count(f.film_id)
FROM   language l
       INNER JOIN film f
               ON l.language_id = f.language_id
GROUP BY l.name; 
```

#### 문제12번) 40편 이상 출연한 영화 배우(actor)는 누구인가요?
- 고객의 id, 주문건수, 총 결제 금액까지 알려주세요.
```SQL
SELECT a.first_name
       ,a.last_name
FROM   (SELECT fa.actor_id
        FROM   film_actor fa
        GROUP BY fa.actor_id
        HAVING count(*) >= 40) AS fa
       INNER JOIN actor a
               ON fa.actor_id = a.actor_id 
```

#### 문제13번) 고객 등급별 고객 수를 구하세요.(대여 금액 혹은 매출액에 따라 고객 등급을 나누고 조건은 아래와 같습니다.)
- A등급은 151 이상
- B등급은 101 이상 150 이하
- C등급은 51 이상 100 이하
- D등급은 50 이하
- 대여 금액의 소수점은 반올림 하세요.(반올림 하는 함수는 ROUND 입니다.)
```SQL
SELECT (CASE
           WHEN amount >= 151 THEN 'A'
           WHEN amount BETWEEN 101 AND 150 THEN 'B'
           WHEN amount BETWEEN 51 AND 100 THEN 'C'
           ELSE 'D'
         END) AS grade,
       count(*)
FROM   (SELECT p.customer_id,
               round(sum(p.amount)) AS amount
        FROM   payment p
        GROUP BY p.customer_id) p
GROUP BY (CASE
              WHEN amount >= 151 THEN 'A'
              WHEN amount BETWEEN 101 AND 150 THEN 'B'
              WHEN amount BETWEEN 51 AND 100 THEN 'C'
              ELSE 'D'
           END) 
	   
-- 까다로웠음..

/*
SELECT 
(CASE 
	WHEN round(sum(p.amount)) >= 151 THEN 'A'
	WHEN round(sum(p.amount)) BETWEEN 101 AND 150 THEN 'B'
	WHEN round(sum(p.amount)) BETWEEN 51 AND 100 THEN 'C'
	ELSE 'D'
END) AS grade
FROM payment p
GROUP BY CASE 
	WHEN round(sum(p.amount)) >= 151 THEN 'A'
	WHEN round(sum(p.amount)) BETWEEN 101 AND 150 THEN 'B'
	WHEN round(sum(p.amount)) BETWEEN 51 AND 100 THEN 'C'
	ELSE 'D'
END)
ORDER BY grade;
*/

-- 처음 시도했던 방식인데 group by에 집계함수 사용할 수 없는 거 생각 안 하고 짰음
-- 고객 등급별 집계를 하려면 select절을 그대로 group by에 사용해야 함
-- select 절에 집계함수 안 쓰는 방법이 없을까 고민해보고 from 절 서브쿼리 활용하기로 결정 !
```
