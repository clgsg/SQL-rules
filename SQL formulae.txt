# Ejercicio SQL - Postgres 🐘

## Antes de empezar 📚

- Recuerda tener los contenedores de `Postgres` y `Adminer` corriendo desde `docker-compose` 🐳
- Desde Adminer (`localhost:8080`) importa y ejecuta los ficheros:
  - `SQL-laureates`
  - `SQL-nobels`

## Consultas 📝

1. Devuelve toda la información de la tabla `laureates`.
 SELECT * FROM laureates
2. Devuelve toda la información de la tabla `nobels`.
 SELECT * FROM nobels

### Nobels

3. Devuelve toda la información relativa al Nobel de Química.

SELECT * FROM nobels WHERE category_fullname ~ 'Chemistry' 
O  
SELECT * FROM nobels WHERE category = 'Chemistry'  

4. Devuelve el nombre completo del Nobel de Ciencias Económicas.

 SELECT category_fullname FROM nobels WHERE category ~ 'Economic' 

5. Devuelve la motivación y la categoría del Nobel de la Paz.
 SELECT motivation, category FROM nobels WHERE category = 'Peace' 

6. Devuelve el nombre completo y el `award_year` de los Nobel dados en el año 2019.
 SELECT category_fullname, award_year FROM nobels WHERE award_year = 2019  


7. Devuelve el la categoría y el `award_year` de los Nobel dados entre las fechas:
  - 11 de octubre de 2019
  - 12 de octubre de 2007

👍 
SELECT category, award_year FROM nobels WHERE date_awarded BETWEEN '2019-10-11' AND '2007-10-12'

8. Devuelve el nombre completo, país y `award_year` de los Nobel de Química ordenados desde el más antiguo al más reciente
  
👍 
SELECT l.full_name, l.birth_country AS country, n.award_year AS year
FROM nobels AS n
INNER JOIN laureates AS l
ON n.laureate_id = l.id
WHERE category LIKE 'Chemistry' ORDER BY award_year  




9. Devuelve la categoría, la motivación y el premio de los Nobel cuyo premio sea mayor de 500000$.

  SELECT category, motivation, prize_amount FROM nobels WHERE prize_amount > 500000  


10. Devuelve la categoría, y el `award_year` y el premio de los Nobel cuya categoría sea Física.
 SELECT category, award_year, prize_amount FROM nobels WHERE category LIKE 'Physics'  


11. Devuelve la categoría y el `award_year` de los Nobel cuyo premio esté comprendido entre 100000$ y 300000$.

👍  SELECT category, award_year FROM nobels WHERE prize_amount BETWEEN 100000 AND 300000



12. Devuelve la suma de los premios Nobel de las categorías de Paz y Literatura.

  SELECT SUM(prize_amount) FROM nobels WHERE category = 'Peace' OR category = 'Literature'




13. Devuelve las 5 afiliaciones y categorías de los Nobel cuyos premios sean los menores.

SELECT affiliations, category FROM nobels ORDER BY prize_amount LIMIT 5



14. Devuelve los 7 `award_year` más recientes en los que se ha entregado algún Nobel. Los años NO deben repetirse. Tienes que devolver 7 diferentes.

SELECT DISTINCT award_year FROM nobels ORDER BY award_year DESC LIMIT 7




15. Devuelve `award_year`, `category_fullname` y `date_awarded` de los Nobel cuyo campo `date_awarded` sea `NULL`.
 SELECT award_year, category_fullname, date_awarded FROM nobels WHERE "date_awarded" IS NULL     -



16. Devuelve la suma de los premios de los Nobel cuyo campo `date_awarded` sea `NULL`
 SELECT SUM(prize_amount) FROM nobels WHERE "date_awarded" IS NULL 
### Laureates

17. Devuelve todos los `known_name` de la tabla `laureates`.

SELECT known_name FROM laureates


18. Devuelve el `known_name` y el año de nacimiento de los 10 laureados más viejos.

SELECT known_name, EXTRACT(YEAR FROM birth_date) FROM "laureates" ORDER BY "birth_date" LIMIT 10



19. Devuelve el `full_name` y el género y la fecha de la muerte de todos los laureados que no sigan vivos.

SELECT full_name, gender, death_date FROM "laureates" WHERE death_date IS NOT null


20. Devuelve el país y el `full_name` de los 10 laureados más jóvenes.

SELECT birth_country, full_name FROM "laureates" ORDER BY "birth_date" DESC LIMIT 10



21. Devuelve `known_name` y ciudad del tercer laureado más jóven.

SELECT known_name, birth_city FROM "laureates" ORDER BY birth_date DESC OFFSET 2 ROWS FETCH NEXT 1 ROWS ONLY;



22. Devuelve todos los países SIN REPETIR cuyo laureado no haya muerto todavía.

SELECT DISTINCT birth_country FROM "laureates" WHERE death_country IS NULL


23. Devuelve la quinta, sexta y séptima ciudad de nacimiento de los laureados ordenadas descendentemente. Si se llegasen a repetir haz que no se repitan.

SELECT DISTINCT birth_city FROM "laureates" ORDER BY birth_city DESC OFFSET 4 ROWS FETCH NEXT 3 ROWS ONLY;


### ✨🎉

24. Devuelve el país de nacimiento, `known_name` y categoría Nobel de los premiados en Química.

SELECT l.birth_country AS country, l.known_name AS name, n.category
FROM nobels AS n
INNER JOIN laureates AS l
ON n.laureate_id = l.id
WHERE category LIKE 'Chemistry'


25. Devuelve la suma de los premios de los laureados cuyo país sean `USA` y `Japan`.

SELECT SUM(n.prize_amount) AS amount
FROM nobels AS n
INNER JOIN laureates AS l
ON n.laureate_id = l.id
WHERE l.birth_country LIKE 'USA' OR l.birth_country LIKE 'Japan'


26. Devuelve `award_year`, categoría, motivación y `known_name` de aquellos que han ganado un Nobel procedentes de India.

SELECT n.award_year, n.category, n.motivation, l.known_name
FROM nobels AS n
INNER JOIN laureates AS l
ON n.laureate_id = l.id
WHERE l.birth_country LIKE 'India'



27. Devuelve la afiliación, categoría y `known_name` de los laureados cuyo género sea `female`.

SELECT n.affiliations, n.category, l.known_name AS name
FROM nobels AS n
INNER JOIN laureates AS l
ON n.laureate_id = l.id
WHERE l.gender LIKE 'female'



28. Devuelve el `known_name`, edad en la que fue premiado y el premio de los 5 laureados más jóvenes al momento de ser condecorado y cuyos premios sean los más altos.
SELECT 
l.known_name AS name, 
(award_year - EXTRACT(YEAR FROM l.birth_date)) AS age, 
n.prize_amount as amount
FROM nobels AS n
INNER JOIN laureates AS l
ON n.laureate_id = l.id
ORDER BY age, amount DESC LIMIT 5




29. Devuelve el `known_name` y el premio de los 5 laureados más jóvenes y cuyos premios sean los más altos.

SELECT 
l.known_name AS name, 
(award_year - EXTRACT(YEAR FROM l.birth_date)) AS age, 
n.prize_amount as amount
FROM nobels AS n
INNER JOIN laureates AS l
ON n.laureate_id = l.id
ORDER BY age, amount DESC LIMIT 5



30. Devuelve el nombre completo, la motivación y el premio de aquellos laureados que no tienen ninguna afiliación.


SELECT l.full_name AS name, n.motivation AS motivation, n.prize_amount as amount
FROM nobels AS n
INNER JOIN laureates AS l
ON n.laureate_id = l.id
WHERE n.affiliations IS NULL


31. Devuelve el nombre completo, fecha de muerte y afiliación de los laureados cuyo nombre no comienza con la letra 'A'.

SELECT l.full_name AS name, l.death_date as died, n.affiliations as affiliation
FROM nobels AS n
INNER JOIN laureates AS l
ON n.laureate_id = l.id
WHERE l.full_name NOT LIKE 'A%'   



32. Devuelve el `known_name`, fecha de muerte, categoría y total de años que hace que murieron aquellos que ganaron el Nobel de Química.

XXXXXXXXXX
SELECT l.known_name AS name, l.death_date AS died,  n.category, datediff(EXTRACT(YEAR FROM death_date),getdate()) as [age 2], (((365* year(getdate()))-(365*(year(GETDATE()))))+ (month(getdate())-month(fechanac))*30
+(day(getdate()) -  day(fechanac)))/365 as [age 3],
FROM nobels AS n
INNER JOIN laureates AS l
ON n.laureate_id = l.id
WHERE n.category LIKE 'Chemistry'

select fechanac, edad, datediff(year,fechanac,getdate()) as [edad 2],
(((365* year(getdate()))-(365*(year(fechanac))))+ (month(getdate())-month(fechanac))*30
+(day(getdate()) -  day(fechanac)))/365 as [edad 3]
 
 from tablaprueba

33. Devuelve `known_name`, `category_fullname` y edad actual de los 5 Nobel cuyo `known_name`sean los más cortos, ordenados desde el más joven hasta el más viejo.




34. Devuelve `known_name`, la edad actual solo en años, la edad de cuando recibió el Nobel y la categoría del Nobel de aquellos laureados que tengan más de 80 años y aún sigan vivos.





35. Devuelve el nombre de la categoría y la suma total de los premios recibidos por categoría ordenados alfabéticamente.
