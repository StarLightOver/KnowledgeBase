Query Operators - Comparison

1. Find all documents where the trip was less than or equal to 70 seconds
   and the usertype was not "Subscriber"
2. Find all documents where the trip was less than or equal to 70 seconds
   and the usertype was "Customer" using a redundant equality operator.
3. Find all documents where the trip was less than or equal to 70 seconds
   and the usertype was "Customer" using the implicit equality operator.

   Операторы запроса - Сравнение

1. Найдите все документы, в которых время поездки было меньше или равно 70 секундам
и тип пользователя не был "Подписчиком".
2. Найдите все документы, в которых время поездки было меньше или равно 70 секундам
и тип пользователя был "Клиент", использующий избыточный оператор равенства.
3. Найдите все документы, в которых время поездки было меньше или равно 70 секундам
и тип пользователя был "Customer" с использованием неявного оператора равенства.

{
  _id: ObjectId("572bb8222b288919b68abf5a"),
  tripduration: 379,
  'start station id': 476,
  'start station name': 'E 31 St & 3 Ave',
  'end station id': 498,
  'end station name': 'Broadway & W 32 St',
  bikeid: 17827,
  usertype: 'Subscriber',
  'birth year': 1969,
  'start station location': { type: 'Point', coordinates: [ -73.97966069, 40.74394314 ] },
  'end station location': { type: 'Point', coordinates: [ -73.98808416, 40.74854862 ] },
  'start time': ISODate("2016-01-01T00:00:45.000Z"),
  'stop time': ISODate("2016-01-01T00:07:04.000Z")
}

1. db.trips.find({tripduration:{$lte:70}, usertype:{$ne:"Subscriber"}})
2. db.trips.find({tripduration:{$lte:70}, usertype:{$eq:"Customer"}})
3. db.trips.find({tripduration:{$lte:70}, usertype:"Customer"})

---------------------------------------------------------------------------------------

Query Operators - Logic

Find all documents where airplanes CR2 or A81 left or landed in the KZN
airport.

Операторы запроса - Логика

Найдите все документы, в которых самолеты CR2 или A81 вылетали или приземлялись в
аэропорту KZN.

{
  _id: ObjectId("56e9b39b732b6122f877fa31"),
  airline: { id: 410, name: 'Aerocondor', alias: '2B', iata: 'ARD' },
  src_airport: 'CEK',
  dst_airport: 'KZN',
  codeshare: '',
  stops: 0,
  airplane: 'CR2'
}

db.routes.find({ $expr:{ $and:[{$eq:["$src_airport","$dst_airport"]},{$or:[{airplane:"CR2"},{airplane:"A81"}]}] } })

---------------------------------------------------------------------------------------

Expressive Query Operator

1. Find all documents where the trip started and ended at the same station.
2. Find all documents where the trip lasted longer than 1200 seconds, and
   started and ended at the same station.

Выразительный оператор Запроса

1. Найдите все документы, в которых поездка начиналась и заканчивалась на одной и той же станции.
2. Найдите все документы, в которых поездка длилась более 1200 секунд и
начиналась и заканчивалась на одной и той же станции.

{
  _id: ObjectId("572bb8222b288919b68abf5a"),
  tripduration: 379,
  'start station id': 476,
  'start station name': 'E 31 St & 3 Ave',
  'end station id': 498,
  'end station name': 'Broadway & W 32 St',
  bikeid: 17827,
  usertype: 'Subscriber',
  'birth year': 1969,
  'start station location': { type: 'Point', coordinates: [ -73.97966069, 40.74394314 ] },
  'end station location': { type: 'Point', coordinates: [ -73.98808416, 40.74854862 ] },
  'start time': ISODate("2016-01-01T00:00:45.000Z"),
  'stop time': ISODate("2016-01-01T00:07:04.000Z")
}

db.trips.find({$expr:{$and:["start station id","end station id"]}})
db.trips.find({$expr:{$and:[{$gt:["$tripduration", 1200]},{$and:["start station id","end station id"]}]}})

---------------------------------------------------------------------------------------

Array Operators

1. Find all documents that contain more than one amenity without caring
   about the order of array elements.
2. Only return documents that list exactly 20 amenities in this field and
   contain the amenities of your choosing.

Операторы массива

1. Найдите все документы, содержащие более одного удобства, не заботясь
о порядке элементов массива.
2. Возвращайте только те документы, в которых в этом поле указано ровно 20 удобств и
которые содержат удобства по вашему выбору.

{
  _id: '10006546',
  listing_url: 'https://www.airbnb.com/rooms/10006546',
  name: 'Ribeira Charming Duplex',
  summary: 'Fantastic duplex ',
  space: 'Privileged views of the oken speech and contagious with your sincere generosity, wrapped in a only parochial spirit',
  description: 'Fantastic duplex ',
  neighborhood_overview: "In the neigh",
  notes: 'Lose yourself ',
  transit: 'Transport: ',
  property_type: 'House',
  room_type: 'Entire home/apt',
  bed_type: 'Real Bed',
  minimum_nights: '2',
  maximum_nights: '30',
  cancellation_policy: 'moderate',
  last_scraped: ISODate("2019-02-16T05:00:00.000Z"),
  calendar_last_scraped: ISODate("2019-02-16T05:00:00.000Z"),
  first_review: ISODate("2016-01-03T05:00:00.000Z"),
  last_review: ISODate("2019-01-20T05:00:00.000Z"),
  accommodates: 8,
  bedrooms: 3,
  beds: 5,
  number_of_reviews: 51,
  bathrooms: Decimal128("1.0"),
  amenities: [
    'TV',
  ],
  price: Decimal128("80.00"),
  security_deposit: Decimal128("200.00"),
  cleaning_fee: Decimal128("35.00"),
  extra_people: Decimal128("15.00"),
  guests_included: Decimal128("6"),
  images: {
    thumbnail_url: '',
    medium_url: '',
    picture_url: 'https://a0.muscache.com/im/pictures/e83e702f-ef49-40fb-8fa0-6512d7e26e9b.jpg?aki_policy=large',
    xl_picture_url: ''
  },
  host: {
    host_id: '51399391',
    host_url: 'https://www.airbnb.com/users/show/51399391',
    host_name: 'Ana&Gonçalo',
    host_location: 'Porto, Porto District, Portugal',
    host_about: 'Gostamos de passear, de viajar, de conhecer pessoas e locais novos, gostamos de desporto e animais! Vivemos na cidade mais linda do mundo!!!',
    host_response_time: 'within an hour',
    host_thumbnail_url: 'https://a0.muscache.com/im/pictures/fab79f25-2e10-4f0f-9711-663cb69dc7d8.jpg?aki_policy=profile_small',
    host_picture_url: 'https://a0.muscache.com/im/pictures/fab79f25-2e10-4f0f-9711-663cb69dc7d8.jpg?aki_policy=profile_x_medium',
    host_neighbourhood: '',
    host_response_rate: 100,
    host_is_superhost: false,
    host_has_profile_pic: true,
    host_identity_verified: true,
    host_listings_count: 3,
    host_total_listings_count: 3,
    host_verifications: [
      'email',
      'phone',
      'reviews',
      'jumio',
      'offline_government_id',
      'government_id'
    ]
  },
  address: {
    street: 'Porto, Porto, Portugal',
    suburb: '',
    government_area: 'Cedofeita, Ildefonso, Sé, Miragaia, Nicolau, Vitória',
    market: 'Porto',
    country: 'Portugal',
    country_code: 'PT',
    location: {
      type: 'Point',
      coordinates: [ -8.61308, 41.1413 ],
      is_location_exact: false
    }
  },
  availability: {
    availability_30: 28,
    availability_60: 47,
    availability_90: 74,
    availability_365: 239
  },
  review_scores: {
    review_scores_accuracy: 9,
    review_scores_cleanliness: 9,
    review_scores_checkin: 10,
    review_scores_communication: 10,
    review_scores_location: 10,
    review_scores_value: 9,
    review_scores_rating: 89
  },
  reviews: [
    {
      _id: '403055315',
      date: ISODate("2019-01-20T05:00:00.000Z"),
      listing_id: '10006546',
      reviewer_id: '15138940',
      reviewer_name: 'Milo',
      comments: 'The house was extremely well located and Ana was able to give us some really great tips on locations to have lunch and eat out. The house was perfectly clean and the easily able to accommodate 6 people despite only having one bathroom. The beds and living room were comfortable. \n' +
        '\n' +
        'However, we always felt somewhat on edge in the house due to the number of signs posted around the kitchen, bedrooms and bathroom about being charged 15€ for all sorts of extras like not washing up or using extra towels and bed linen. Not that this would be particularly unreasonable but it made us feel like we were walking on egg shells in and around the house. \n' +
        '\n' +
        "The hosts were aware that we were a group of six yet one of the beds was not prepared and we ran out of toilet paper well before we were due to check out despite only being there 2 nights. It really wasn't the end of the world but the shower head does not have a wall fitting meaning you had to hold it yourself if you wanted to stand underneath it."
    }
  ]
}

db.listingsAndReviews.find({amenities:{$gt:["$size",1]}})
db.listingsAndReviews.find({amenities:{
	"$size": 20,
	$all:["TV","Internet"]
}})

---------------------------------------------------------------------------------------

Array Operators and Projection

1. Find all documents in the sample_airbnb database with exactly 20
   amenities which include all the amenities listed in the query array, and display their price and address.
2. Find all documents in the sample_airbnb database that have Wifi as one of
   the amenities only include price and address in the resulting cursor.
3. Find all documents in the sample_airbnb database that have Wifi as one of
   the amenities only include price and address in the resulting cursor,
   also exclude "maximum_nights".
   Was this operation successful? Why?
4. Find all documents in the grades collection where the student in class
   431 received a grade higher than 85 for any type of assignment.
5. Find all documents in the grades collection where the student had an
   extra credit score.

Операторы массива и проекция

1. Найдите все документы в базе данных sample_airbnb с ровно 20
удобствами, которые включают все удобства, перечисленные в массиве запросов, и отобразите их цену и адрес.
2. Найдите все документы в базе данных sample_airbnb, в которых Wi-Fi является одним из
удобств, укажите только цену и адрес в результирующем курсоре.
3. Найдите все документы в базе данных sample_airbnb, в которых Wi-Fi является одним из
удобств, укажите только цену и адрес в результирующем курсоре,
также исключите "maximum_nights".
Была ли эта операция успешной? Почему?
4. Найдите все документы в коллекции оценок, где учащийся в классе
431 получил оценку выше 85 за любой тип задания.
5. Найдите все документы в коллекции оценок, в которых учащийся имел
дополнительный кредитный балл.

use sample_airbnb

db.listingsAndReviews.find({amenities:{
	"$size": 20,
	$all:["TV","Internet"]
}},
{price:1,address:1})

db.listingsAndReviews.find({amenities:{
	"$size": 20,
	$all:["Wifi"]
}},
{price:1,address:1})

db.listingsAndReviews.find({amenities:{
	"$size": 20,
	$all:["Wifi"]
}},
{price:1,address:1,maximum_nights:0})

use sample_training

{
  _id: ObjectId("56d5f7eb604eb380b0d8d8ce"),
  student_id: 0,
  scores: [
    { type: 'exam', score: 78.40446309504266 },
    { type: 'quiz', score: 73.36224783231339 },
    { type: 'homework', score: 46.980982486720535 },
    { type: 'homework', score: 76.67556138656222 }
  ],
  class_id: 339
}

db.grades.find({
	class_id:431,
	scores:{
		$elemMatch:{score:{$gt:85}}
	}
})

db.grades.find({
	scores:{
		$elemMatch:{type:"extra credit"}
	}
})

---------------------------------------------------------------------------------------

Array Operators and Sub-Documents

1. Find any document from the companies collection where the last name
   Zuckerberg in the first element of the relationships array.
2. Find how many documents from the companies collection have CEOs who's
   first name is Mark and who are listed as the first relationship in this
   array for their company entry.
3. Find all documents in the companies collection where people named Mark
   used to be in the senior company leadership array, a.k.a the
   relationships array, but are no longer with the company.

Операторы массива и вложенные документы

1. Найдите любой документ из коллекции компаний, в котором фамилия
Цукерберга в первом элементе массива взаимосвязей.
2. Найдите, сколько документов из коллекции компаний содержат генеральных директоров, которых зовут
Марк и которые указаны в качестве первых отношений в этом
массиве для их записи о компании.
3. Найдите все документы в коллекции компаний, где люди с именем Марк
раньше входил в группу высшего руководства компании
, также известную как группа взаимоотношений, но больше не работает в компании.

db.companies.find({"relationships.0.person.last_name": "Zuckerberg"})

db.companies.find({"relationships.0.person.first_name": "Mark", "relationships.0.title":"CEO"}).count()

db.companies.find({"relationships":{$elemMatch:{"person.first_name":"Mark", is_past:true}}})








