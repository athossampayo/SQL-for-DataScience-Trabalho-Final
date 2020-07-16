# Data Scientist Role Play: Profiling and Analyzing the Yelp Dataset Coursera Worksheet

This is a 2-part assignment. In the first part, you are asked a series of questions that will help you profile and understand the data just like a data scientist would. For this first part of the assignment, you will be assessed both on the correctness of your findings, as well as the code you used to arrive at your answer. You will be graded on how easy your code is to read, so remember to use proper formatting and comments where necessary.

In the second part of the assignment, you are asked to come up with your own inferences and analysis of the data for a particular research question you want to answer. You will be required to prepare the dataset for the analysis you choose to do. As with the first part, you will be graded, in part, on how easy your code is to read, so use proper formatting and comments to illustrate and communicate your intent as required.

For both parts of this assignment, use this "worksheet." It provides all the questions you are being asked, and your job will be to transfer your answers and SQL coding where indicated into this worksheet so that your peers can review your work. You should be able to use any Text Editor (Windows Notepad, Apple TextEdit, Notepad ++, Sublime Text, etc.) to copy and paste your answers. If you are going to use Word or some other page layout application, just be careful to make sure your answers and code are lined appropriately.
In this case, you may want to save as a PDF to ensure your formatting remains intact for you reviewer.



## Part 1: Yelp Dataset Profiling and Understanding

**1. Profile the data by finding the total number of records for each of the tables below:**
<br>
	```SELECT COUNT(*)
	FROM table```
<br>
i. Attribute table = 10000 
ii. Business table = 10000
iii. Category table = 10000 
iv. Checkin table = 10000 
v. elite_years table = 10000 
vi. friend table =  10000 
vii. hours table = 10000 
viii. photo table = 10000 
ix. review table = 10000 
x. tip table = 10000 
xi. user table = 10000 
	


**2. Find the total distinct records by either the foreign key or primary key for each table. If two foreign keys are listed in the table, please specify which foreign key.**

	SELECT COUNT(DISTINCT key_column)
	FROM table

i. Business = 10000 (id) <br>
ii. Hours = 1562 (business_id) <br>
iii. Category = 2643 (business_id) <br>
iv. Attribute = 1115 (business_id) <br>
v. Review = 10000 (id) | 8090 (business_id) | 9581 (user_id) <br>
vi. Checkin = 493 (business_id) <br>
vii. Photo = 10000 (id) | 6493 (business_id) <br>
viii. Tip = 537 (user_id) | 3979 (business_id) <br>
ix. User = 10000  <br>
x. Friend = 11 (user_id) <br>
xi. Elite_years = 2780 (user_id) <br>

Note: Primary Keys are denoted in the ER-Diagram with a yellow key icon.	



**3. Are there any columns with null values in the Users table? Indicate "yes," or "no."**

Answer: NO
	
	
SQL code used to arrive at answer:

	SELECT id,
	SUM(CASE WHEN IS NULL THEN 1 END)
	FROM user

Used this code bellow to double check:

	SELECT id,
	COUNT(1) - COUNT(2),
	COUNT(1) - COUNT(3),
	COUNT(1) - COUNT(4),
	COUNT(1) - COUNT(5),
	COUNT(1) - COUNT(6),
	COUNT(1) - COUNT(7),
	COUNT(1) - COUNT(8),
	COUNT(1) - COUNT(9),
	COUNT(1) - COUNT(10),
	COUNT(1) - COUNT(11),
	COUNT(1) - COUNT(12),
	COUNT(1) - COUNT(13),
	COUNT(1) - COUNT(14),
	COUNT(1) - COUNT(15),
	COUNT(1) - COUNT(16),
	COUNT(1) - COUNT(17),
	COUNT(1) - COUNT(18),
	COUNT(1) - COUNT(19),
	COUNT(1) - COUNT(20)
	FROM user
	
	
**4. For each table and column listed below, display the smallest (minimum), largest (maximum), and average (mean) value for the following fields:**

i. Table: Review, Column: Stars
	
min: 1		max: 5		avg: 3.7082
		SELECT MIN(stars),
		MAX(stars),
		AVG(stars)
		FROM Review
	
ii. Table: Business, Column: Stars
	
min: 1.0	max: 5.0	avg: 3.6549 
		SELECT MIN(stars),
		MAX(stars),
		AVG(stars)
		FROM Business
	
iii. Table: Tip, Column: Likes
	
min: 0		max: 2		avg: 0.0144 
		SELECT MIN(likes),
		MAX(likes),
		AVG(likes)
		FROM Tip

iv. Table: Checkin, Column: Count
	
min: 1		max: 53		avg: 1.9414 
		SELECT MIN(count),
		MAX(count),
		AVG(count)
		FROM Checkin
	
v. Table: User, Column: Review_count
	
min: 0		max: 2000	avg: 24.2995 
		SELECT MIN(Review_count),
		MAX(Review_count),
		AVG(Review_count)
		FROM User



**5. List the cities with the most reviews in descending order:**

SQL code used to arrive at answer:
	SELECT city,
	SUM(review_count)
	FROM business
	GROUP BY City
	ORDER BY review_count DESC

**PS: I tried to fix the wrong results by using this solution but there are no write permissions on this database.
	https://www.enavigo.com/2011/08/02/sqlite-order-by-does-not-work-on-integers-time-for-an-index/

Copy and Paste the Result Below:
	+------------------------+-------------------+
	| city                   | SUM(review_count) |
	+------------------------+-------------------+
	| Woodmere Village       |               242 |
	| Mount Lebanon          |               138 |
	| Charlotte              |             12523 |
	| McMurray               |               116 |
	| North York             |               998 |
	| Enterprise             |                89 |
	| Matthews               |               773 |
	| Munroe Falls           |                74 |
	| Ahwatukee              |               235 |
	| Pittsburgh             |              9798 |
	| Woodmere               |               104 |
	| Tolleson               |               113 |
	| Pineville              |               563 |
	| Carnegie               |                72 |
	| Macedonia              |                71 |
	| Markham                |              2352 |
	| Whitchurch-Stouffville |                52 |
	| Windsor                |                50 |
	| Goodyear               |              1155 |
	| Gibsonia               |                70 |
	| Summerlin              |                44 |
	| Peninsula              |                42 |
	| Richfield              |                64 |
	| Dormont                |                40 |
	| nboulder city          |                40 |
	+------------------------+-------------------+
	(Output limit exceeded, 25 of 362 total rows shown)

	
**6. Find the distribution of star ratings to the business in the following cities:**

i. Avon

SQL code used to arrive at answer:
	SELECT stars,
	SUM(review_count) AS Distribution
	FROM business
	WHERE city='Avon'
	GROUP BY stars

Copy and Paste the Resulting Table Below (2 columns – star rating and count):
	```
	+-------+--------------+
	| stars | Distribution |
	+-------+--------------+
	|   1.5 |           10 |
	|   2.5 |            6 |
	|   3.5 |           88 |
	|   4.0 |           21 |
	|   4.5 |           31 |
	|   5.0 |            3 |
	+-------+--------------+
	```

ii. Beachwood

SQL code used to arrive at answer:
	```
	SELECT stars,
	SUM(review_count) AS Distribution
	FROM business
	WHERE city='Beachwood'
	GROUP BY stars```

Copy and Paste the Resulting Table Below (2 columns – star rating and count):
	```
	+-------+--------------+
	| stars | Distribution |
	+-------+--------------+
	|   2.0 |            8 |
	|   2.5 |            3 |
	|   3.0 |           11 |
	|   3.5 |            6 |
	|   4.0 |           69 |
	|   4.5 |           17 |
	|   5.0 |           23 |
	+-------+--------------+
	```


**7. Find the top 3 users based on their total number of reviews:**
		
SQL code used to arrive at answer:
	```SELECT id,
	name,
	review_count,
	fans
	FROM user
	ORDER BY review_count DESC```				
		
Copy and Paste the Result Below:
	```
	+------------------------+-----------+--------------+------+
	| id                     | name      | review_count | fans |
	+------------------------+-----------+--------------+------+
	| -G7Zkl1wIWBBmD0KRy_sCw | Gerald    |         2000 |  253 |
	| -3s52C4zL_DHRK0ULG6qtg | Sara      |         1629 |   50 |
	| -8lbUNlXVSoXqaRRiHiSNg | Yuri      |         1339 |   76 |
	| -K2Tcgh2EKX6e6HqqIrBIQ | .Hon      |         1246 |  101 |
	| -FZBTkAZEXoP7CYvRV2ZwQ | William   |         1215 |  126 |
	| --2vR0DIsmQ6WfcSzKWigw | Harald    |         1153 |  311 |
	| -gokwePdbXjfS0iF7NsUGA | eric      |         1116 |   16 |
	| -DFCC64NXgqrxlO8aLU5rg | Roanna    |         1039 |  104 |
	```


**8. Does posing more reviews correlate with more fans?**

Please explain your findings and interpretation of the results:
_No, as seen in the result above, fans count does not directly correlate to reviews_count column._

	
**9. Are there more reviews with the word "love" or with the word "hate" in them?**

Answer:
_There are more reviews with the love word._
	```
	+---------------+---------------+
	| Has_Love_word | Has_Hate_word |
	+---------------+---------------+
	|          1780 |           232 |
	+---------------+---------------+
	```
	
SQL code used to arrive at answer:

	SELECT
	COUNT((CASE
	WHEN r.text LIKE ('%love%') THEN 1
	END)) AS Has_Love_word,
	COUNT((CASE
	WHEN r.text LIKE ('%hate%') THEN 1
	END)) AS Has_Hate_word
	FROM review AS r
	
**10. Find the top 10 users with the most fans:**

SQL code used to arrive at answer:
	SELECT id,
	name,
	fans
	FROM user
	ORDER BY fans DESC
	LIMIT 10
	
Copy and Paste the Result Below:
	```
	+------------------------+-----------+------+
	| id                     | name      | fans |
	+------------------------+-----------+------+
	| -9I98YbNQnLdAmcYfb324Q | Amy       |  503 |
	| -8EnCioUmDygAbsYZmTeRQ | Mimi      |  497 |
	| --2vR0DIsmQ6WfcSzKWigw | Harald    |  311 |
	| -G7Zkl1wIWBBmD0KRy_sCw | Gerald    |  253 |
	| -0IiMAZI2SsQ7VmyzJjokQ | Christine |  173 |
	| -g3XIcCb2b-BD0QBCcq2Sw | Lisa      |  159 |
	| -9bbDysuiWeo2VShFJJtcw | Cat       |  133 |
	| -FZBTkAZEXoP7CYvRV2ZwQ | William   |  126 |
	| -9da1xk7zgnnfO1uTVYGkA | Fran      |  124 |
	| -lh59ko3dxChBSZ9U7LfUw | Lissa     |  120 |
	+------------------------+-----------+------+
	```
		

## Part 2: Inferences and Analysis

**1. Pick one city and category of your choice and group the businesses in that city or category by their overall star rating. Compare the businesses with 2-3 stars to the businesses with 4-5 stars and answer the following questions. Include your code.**
	
_I chose Toronto and the Restaurants category._ <br>
i. Do the two groups you chose to analyze have a different distribution of hours? <br>
_Yes, the 2-3 stars group only opens Monday and Tuesday, but the 4-5 stars group opens from Tuesday to Sunday._ <br>
SQL code used for analysis: <br>
	```
	SELECT B.city,
	B.stars,
	CASE
	WHEN H.hours LIKE '%sunday%' THEN 1
	WHEN H.hours LIKE '%monday%' THEN 2
	WHEN H.hours LIKE '%tuesday%' THEN 3
	WHEN H.hours LIKE '%wednesday%' THEN 4
	WHEN H.hours LIKE '%thursday%' THEN 5
	WHEN H.hours LIKE '%friday%' THEN 6
	WHEN H.hours LIKE '%saturday%' THEN 7
	END AS week_days,
	AVG(B.review_count),
	SUM(B.review_count),
	COUNT(B.id),
	CASE
	WHEN B.stars BETWEEN 2 AND 3 THEN '2-3 stars'
	WHEN B.stars BETWEEN 4 AND 5 THEN '4-5 stars'
	END AS star_rating
	FROM business B INNER JOIN hours H 
	ON B.id = H.business_id
	INNER JOIN category C
	on B.id = C.business_id
	WHERE (city = 'Toronto'
	AND
	category LIKE 'Restaurants'
	AND
	star_rating IS NOT NULL)
	GROUP BY week_days
	ORDER BY star_rating
	``` <br>
ii. Do the two groups you chose to analyze have a different number of reviews? <br>
_Yes, the first group (2-3 stars) have 86 reviews, with an avarage of 86 reviews; and the second group (4-5 stars), have total of 206 reviews and an avarage of 41.2._
<br> SQL code used for analysis:
	```
	SELECT B.id,
	B.city,
	B.stars,
	AVG(B.review_count),
	SUM(B.review_count),
	COUNT(B.id),
	CASE
	WHEN B.stars BETWEEN 2 AND 3 THEN '2-3 stars'
	WHEN B.stars BETWEEN 4 AND 5 THEN '4-5 stars'
	END AS stars_groups
	FROM business B INNER JOIN category C
	on B.id = C.business_id
	WHERE (city = 'Toronto'
	AND
	category LIKE 'Restaurants'
	AND
	stars_groups IS NOT NULL)
	GROUP BY stars_groups
	ORDER BY stars_groups
	``` <br>
iii. Are you able to infer anything from the location data provided between these two groups? Explain. <br>
_Yes. The 2-3 stars group are only localized in two neighborhoods: Downtown Core (with 2 out of 3 business) and Entertainment District. The 4-5 stars group is more diverse, being distributed in 5 neighborhoods._ <br>
SQL code used for analysis: <br>
	```
	SELECT B.id,
	B.city,
	B.stars,
	B.neighborhood,
	AVG(B.review_count),
	SUM(B.review_count),
	COUNT(B.id),
	CASE
	WHEN B.stars BETWEEN 2 AND 3 THEN '2 and 3 stars'
	WHEN B.stars BETWEEN 4 AND 5 THEN '4 and 5 stars'
	END AS stars_groups
	FROM business B INNER JOIN category C
	on B.id = C.business_id
	WHERE (city = 'Toronto'
	AND
	category LIKE 'Restaurants'
	AND
	stars_groups IS NOT NULL)
	GROUP BY B.neighborhood
	ORDER BY stars_groups
	``` <br>
		
		
**2. Group business based on the ones that are open and the ones that are closed. What differences can you find between the ones that are still open and the ones that are closed? List at least two differences and the SQL code you used to arrive at your answer.**
Result Below:
		```
		+----------+-------+-----------+---------------------+---------------------+-------------+---------+
		| city     | stars | week_days | AVG(B.review_count) | SUM(B.review_count) | COUNT(B.id) | is_open |
		+----------+-------+-----------+---------------------+---------------------+-------------+---------+
		| Montréal |   3.0 |         7 |       51.0514184397 |               28793 |         564 |       0 |
		| Toronto  |   4.0 |         7 |       72.9368252699 |              182415 |        2501 |       1 |
		+----------+-------+-----------+---------------------+---------------------+-------------+---------+ ``` <br>
i. Difference 1:
 _There are ~4.43% more business open than closed. (2051 open in comparison to 564 closed)_
         
ii. Difference 2:
_The AVG of reviews is greater in open business, even though it is only ~1.43% greater. (72.94 open compared to 51.05 closed)_
         
         
SQL code used for analysis:

	SELECT B.city,
	B.stars,
	CASE
	WHEN H.hours LIKE '%sunday%' THEN 1
	WHEN H.hours LIKE '%monday%' THEN 2
	WHEN H.hours LIKE '%tuesday%' THEN 3
	WHEN H.hours LIKE '%wednesday%' THEN 4
	WHEN H.hours LIKE '%thursday%' THEN 5
	WHEN H.hours LIKE '%friday%' THEN 6
	WHEN H.hours LIKE '%saturday%' THEN 7
	END AS week_days,
	AVG(B.review_count),
	SUM(B.review_count),
	COUNT(B.id),
	B.is_open
	FROM business B INNER JOIN hours H 
	ON B.id = H.business_id
	INNER JOIN category C
	on B.id = C.business_id
	GROUP BY B.is_open
	
**3. For this last part of your analysis, you are going to choose the type of analysis you want to conduct on the Yelp dataset and are going to prepare the data for analysis.**

Ideas for analysis include: Parsing out keywords and business attributes for sentiment analysis, clustering businesses to find commonalities or anomalies between them, predicting the overall star rating for a business, predicting the number of fans a user will have, and so on. These are just a few examples to get you started, so feel free to be creative and come up with your own problem you want to solve. Provide answers, in-line, to all of the following:
	
	
i. Indicate the type of analysis you chose to do:
_We would like to predict which of the attributes are most important for greater star rating. Could also finish with a sentiment analysis using a simple dictionary for tagging user texts and their feeling about the business._
        
ii. Write 1-2 brief paragraphs on the type of data you will need for your analysis and why you chose that data:
_To help improve the understanding of attributes needed for better user reviews, this analysis will need most as the business information from Business Table (id, neighborhood, city, stars and review_count) for basic correlations and star/rating information, also, we would use the neighborhood as an attribute for later analysis. We would also need the whole Attribute table, using this data as the major key to identifing important attributes related to better ratings. For the sentiment analysis, would need to pull the 'text' column from the Review Table._
iii. Output of your finished dataset:
```	+------------------------+-----------------+-----------+-------+--------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
	| id                     | neighborhood    | city      | stars | review_count | name                     | value                                                                                                                                           |
	+------------------------+-----------------+-----------+-------+--------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | RestaurantsTableService  | 1                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | GoodForMeal              | {"dessert": false, "latenight": false, "lunch": true, "dinner": true, "breakfast": false, "brunch": false}                                      |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | Alcohol                  | beer_and_wine                                                                                                                                   |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | Caters                   | 0                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | HasTV                    | 0                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | RestaurantsGoodForGroups | 1                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | NoiseLevel               | average                                                                                                                                         |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | WiFi                     | no                                                                                                                                              |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | RestaurantsAttire        | casual                                                                                                                                          |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | RestaurantsReservations  | 1                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | OutdoorSeating           | 0                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | RestaurantsPriceRange2   | 2                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | BikeParking              | 1                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | RestaurantsDelivery      | 0                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | Ambience                 | {"romantic": false, "intimate": false, "classy": false, "hipster": false, "touristy": false, "trendy": false, "upscale": false, "casual": true} |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | RestaurantsTakeOut       | 1                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | GoodForKids              | 1                                                                                                                                               |
	| -0DET7VdEQOJVJ_v6klEug | Brown's Corners | Markham   |   3.0 |           25 | BusinessParking          | {"garage": false, "street": false, "validated": false, "lot": false, "valet": false}                                                            |
	| -2bYV9zVtn2F5XpiAaHt5A |                 | Edinburgh |   3.0 |            4 | GoodForMeal              | {"dessert": false, "latenight": false, "lunch": false, "dinner": false, "breakfast": false, "brunch": false}                                    |
	| -2bYV9zVtn2F5XpiAaHt5A |                 | Edinburgh |   3.0 |            4 | Alcohol                  | none                                                                                                                                            |
	| -2bYV9zVtn2F5XpiAaHt5A |                 | Edinburgh |   3.0 |            4 | HasTV                    | 0                                                                                                                                               |
	| -2bYV9zVtn2F5XpiAaHt5A |                 | Edinburgh |   3.0 |            4 | RestaurantsGoodForGroups | 1                                                                                                                                               |
	| -2bYV9zVtn2F5XpiAaHt5A |                 | Edinburgh |   3.0 |            4 | NoiseLevel               | average                                                                                                                                         |
	| -2bYV9zVtn2F5XpiAaHt5A |                 | Edinburgh |   3.0 |            4 | RestaurantsAttire        | casual                                                                                                                                          |
	| -2bYV9zVtn2F5XpiAaHt5A |                 | Edinburgh |   3.0 |            4 | RestaurantsReservations  | 0                                                                                                                                               |
	+------------------------+-----------------+-----------+-------+--------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------+
	(Output limit exceeded, 25 of 787 total rows shown)```
iv. Provide the SQL code you used to create your final dataset:
	SELECT b.id,
	b.neighborhood,
	b.city,
	b.stars,
	b.review_count,
	a.name,
	a.value
	FROM Business b INNER JOIN Attribute a
	ON b.id = a.business_id


	* For the sentiment analysis, would be needed to do the following changes, but beware that it reduces the output drasticaly, making this only an experimental analysis:
	SELECT b.id,
	b.neighborhood,
	b.city,
	b.stars,
	b.review_count,
	a.name,
	a.value,
	r.text
	FROM Business b INNER JOIN Attribute a
	ON b.id = a.business_id
	INNER JOIN Review r
	ON b.id = r.business_id
