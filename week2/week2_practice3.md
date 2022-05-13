# Exercise

#### 문제1번) 180분 이상의 길이의 영화에 출연하거나 rating이 R인 등급에 해당하는 영화에 출연한 영화 배우에 대해서 영화 배우 ID와 (180분이상/R등급영화)에 대한 Flag컬럼을 알려주세요.
- film_actor 테이블과 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.
- actor_id가 동일한 flag 값이 여러 개 나오지 않도록 해주세요.
```SQL
SELECT actor_id
       ,'over_length' AS flag
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  length >= 180)
UNION
SELECT actor_id
       ,'rating' AS flag
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  rating = 'R');
```

#### 문제2번) R등급의 영화에 출연했던 배우이면서 동시에 Alone Trip의 영화에 출연한 영화배우의 ID를 확인해주세요.
- film_actor 테이블와 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.
```SQL
SELECT actor_id
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  rating = 'R')
INTERSECT
SELECT actor_id
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  title = 'Alone Trip'); 
```

#### 문제3번) G등급에 해당하는 필름을 찍었으나 영화를 20편이상 찍지 않은 영화배우의 ID를 확인해주세요.
- film_actor 테이블와 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.
```SQL
SELECT actor_id
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  rating = 'G')
INTERSECT
SELECT actor_id
FROM   film_actor fa
GROUP BY actor_id
HAVING count(DISTINCT film_id) < 20
ORDER BY actor_id; 
```

#### 문제4번) 필름 중에서 필름 카테고리가 Action, Animation, Horror에 해당하지 않는 필름 아이디를 알려주세요.
- category 테이블을 이용해서 알려주세요.
```SQL
SELECT f.film_id
FROM   film f
EXCEPT
SELECT fc.film_id
FROM   film_category fc
WHERE  fc.category_id IN (SELECT c.category_id
                          FROM   category c
                          WHERE  c.name IN ( 'Action', 'Animation', 'Horror' )
                         ); 
```

#### 문제5번) Staff의 id, 이름, 성에 대한 데이터와 Customer의 id, 이름, 성에 대한 데이터를 하나의 데이터셋의 형태로 보여주세요.
- 컬럼 구성 : id, 이름, 성, flag(직원/고객여부)로 구성해주세요.
```SQL
SELECT s.staff_id   AS id
       ,s.first_name AS 이름
       ,s.last_name  AS 성
       ,(CASE
         WHEN s.staff_id IN (SELECT staff_id
                             FROM   staff) THEN '직원'
         ELSE '고객'
        END) AS flag
FROM   staff s
UNION
SELECT c.customer_id
       ,c.first_name
       ,c.last_name
       ,(CASE
         WHEN c.customer_id IN (SELECT customer_id
                                FROM   customer) THEN '고객'
         ELSE '직원'
        END) AS flag
FROM   customer c
ORDER BY id; 

-- 너무 복잡하게 생각했음. 좀 더 가볍고 단순하게 생각하기 ! 

/* 간단한 버전,,
SELECT staff_id,
       first_name,
       last_name,
       'Staff' AS flag
FROM   staff
UNION ALL
SELECT customer_id,
       first_name,
       last_name,
       'Customer' AS flag
FROM   customer
*/
```

#### 문제6번) 직원과 고객의 이름이 동일한 사람이 혹시 있나요? 있다면 해당 사람의 이름과 성을 알려주세요.
```SQL
SELECT s.first_name
       ,s.last_name
FROM   staff s
INTERSECT
SELECT c.first_name
       ,c.last_name
FROM   customer c; 
```

#### 문제7번) 반납이 되지 않은 대여점(store)별 영화 재고(inventory)와 전체 영화 재고를 같이 구하세요.(union all)
```SQL
SELECT '1 & 2' AS store_id
       ,count(*) AS not_returned_total
       ,(SELECT count(*) FROM rental) AS inventory_total
FROM   rental r
       INNER JOIN inventory i
               ON i.inventory_id = r.inventory_id
WHERE  r.return_date IS NULL
UNION ALL
SELECT i.store_id::text
       ,count(*) AS not_returned_total
       ,(SELECT CASE
                 WHEN i.store_id = 1 THEN (SELECT count(*)
                                           FROM   inventory i
                                                  INNER JOIN rental r
                                                          ON i.inventory_id = r.inventory_id
                                           GROUP BY i.store_id
                                           HAVING i.store_id = 1)
                 ELSE (SELECT count(*)
                       FROM   inventory i
                              INNER JOIN rental r
                                      ON i.inventory_id = r.inventory_id
                       GROUP BY i.store_id
                       HAVING i.store_id = 2)
                END) AS inventory_total
FROM   inventory i
       INNER JOIN rental r
               ON i.inventory_id = r.inventory_id
WHERE  r.return_date IS NULL
GROUP BY i.store_id; 
```

#### 문제8번) 국가(country)별, 도시(city)별 매출액 국가(country)매출액 소계 그리고 전체 매출액을 구하세요.(union all)
```SQL
SELECT c3.country
       ,c2.city
       ,sum(p.amount)
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON c.address_id = a.address_id
       INNER JOIN city c2
               ON a.city_id = c2.city_id
       INNER JOIN country c3
               ON c2.country_id = c3.country_id
GROUP BY c3.country, c2.city
UNION ALL
SELECT c3.country
       ,NULL AS NULL
       ,sum(p.amount)
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON c.address_id = a.address_id
       INNER JOIN city c2
               ON a.city_id = c2.city_id
       INNER JOIN country c3
               ON c2.country_id = c3.country_id
GROUP BY c3.country
UNION ALL
SELECT NULL AS NULL
       ,NULL AS NULL
       ,sum(p.amount)
FROM   payment p
       INNER JOIN customer c
               ON p.customer_id = c.customer_id
       INNER JOIN address a
               ON c.address_id = a.address_id
       INNER JOIN city c2
               ON a.city_id = c2.city_id
       INNER JOIN country c3
               ON c2.country_id = c3.country_id
ORDER BY country, city; 
```
