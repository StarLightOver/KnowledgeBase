# lec 1

use sample_airbnb

db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1, "_id": 0 }).pretty()

db.listingsAndReviews.aggregate([
                                  { "$match": { "amenities": "Wifi" } },
                                  { "$project": { "price": 1,
                                                  "address": 1,
                                                  "_id": 0 }}]).pretty()

db.listingsAndReviews.findOne({ },{ "address": 1, "_id": 0 })

db.listingsAndReviews.aggregate([ { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country" }}])

db.listingsAndReviews.aggregate([
                                  { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country",
                                                "count": { "$sum": 1 } } }
                                ])

# lab 1

Atlas atlas-ngwuoh-shard-0 [primary] sample_airbnb> db.listingsAndReviews.aggregate([{$project:{room_type:1, _id:0}},{$group:{_id:"$room_type"}}])
[
{ _id: 'Entire home/apt' },
{ _id: 'Private room' },
{ _id: 'Shared room' }
]

# lec 2

use sample_training

db.zips.find().sort({ "pop": 1 }).limit(1)

db.zips.find({ "pop": 0 }).count()

db.zips.find().sort({ "pop": -1 }).limit(1)

db.zips.find().sort({ "pop": -1 }).limit(10)

db.zips.find().sort({ "pop": 1, "city": -1 })

# end

Aggregation Framework

## Aggregation Framework

1. Using the aggregation framework find all documents that have Wifi as one
   of the amenities. Only include price and address in the resulting cursor.

2. Which countries have listings in the sample_airbnb database?
3. How many countries have listings in the sample_airbnb database?


1. Используя платформу агрегации, найдите все документы, в которых Wi-Fi является одним
   из удобств. Укажите только цену и адрес в результирующем курсоре.

2. В каких странах есть списки в базе данных sample_airbnb?
3. В скольких странах есть списки в базе данных sample_airbnb?

use sample_airbnb
db.listingsAndReviews.aggregate([{ "$match": { "amenities": "Wifi" } },{ "$project": { "price": 1,"address": 1,"_id": 0 }}])

db.listingsAndReviews.aggregate([ { "$project": { "address": 1, "_id": 0 }},{ "$group": { "_id": "$address.country" }}])

db.listingsAndReviews.aggregate([{ "$project": { "address": 1, "_id": 0 }},{ "$group": { "_id": "$address.country","count": { "$sum": 1 } } }])

## sort() and limit()

1. Find the least populated ZIP code in the zips collection.
2. Find the most populated ZIP code in the zips collection.
3. Find the top ten most populated ZIP codes.
4. Get results sorted in increasing order by population, and decreasing
   order by city name.


1. Найдите наименее заполненный почтовый индекс в коллекции ships.
2. Найдите наиболее населенный почтовый индекс в коллекции обращений.
3. Найдите десятку самых населенных почтовых индексов.
4. Отсортируйте результаты в порядке возрастания по численности населения и
   в порядке убывания по названию города.

db.zips.find().sort({ "pop": 1 }).limit(1)
db.zips.find().sort({ "pop": -1 }).limit(1)
db.zips.find().sort({ "pop": -1 }).limit(10)
db.zips.find().sort({ "pop": 1, "city": -1 })

## Introduction to Indexes

Create two separate indxes to support the following queries:

db.trips.find({"birth year": 1989})

db.trips.find({"start station id": 476}).sort("birth year": 1)

Создайте два отдельных индекса для поддержки следующих запросов:

db.trips.find({"год рождения": 1989})

db.trips.find({"идентификатор начальной станции": 476}).tort("год рождения": 1)

db.trips.createIndex({ "birth year": 1 })

db.trips.createIndex({ "start station id": 1, "birth year": 1 })






