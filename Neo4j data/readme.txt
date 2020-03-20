step1: run querry 1. And you will get an error, its okay, read the error, find the file location, copy all five files into that location.

step2: run querry 1 to 9, one at a time

* if got an error like " *9/8/2015 not a date", open csv file then change the date column format to date type. (ex. 2014-02-01)
1.
LOAD CSV with headers FROM "file:///ranking.csv" AS line
CREATE (a:ranking {store_app_id:line.store_app_id, rank_type:line.rank_type, rank:line.rank, date:date(line.date)})

2.
LOAD CSV with headers FROM "file:///releasenote.csv" AS line
CREATE (a:releasenote { store_app_id:line.store_app_id, body:line.body, version:line.version,date:date(line.date) })

3.
LOAD CSV with headers FROM "file:///app.csv" AS line
CREATE (a:app { store_app_id:line.store_app_id, app_name:line. app_name, category:line.category })

4.
LOAD CSV with headers FROM "file:///review.csv" AS line
CREATE (a:review { store_app_id:line.store_app_id,review_id:line.review_id, title:line.title, body:line.body, star_rating:line.star_rating,date:date(line.date),username:line.username, user_apple_id:line.user_apple_id, user_reviews_url:line.user_reviews_url })

5.
LOAD CSV with headers FROM "file:///reviewer.csv" AS line
CREATE (a:reviewer { username:line.username, user_apple_id:line.user_apple_id, user_reviews_url:line.user_reviews_url })

6.
match (a:app),(b:releasenote)
where a.store_app_id=b.store_app_id
merge (a)-[:has]->(b)

7.
match (a:app),(c:ranking)
where a.store_app_id=c.store_app_id
merge (a)-[:has]->(c)

8.
match (a:app),(d:review)
where a.store_app_id=d.store_app_id
merge (a)-[:has]->(d)

9.
match (d:review),(e:reviewer)
where d.user_apple_id=e.user_apple_id
merge (e)-[:wrote]->(d)
