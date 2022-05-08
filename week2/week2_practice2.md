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

#### 문제5번) 진행중 
