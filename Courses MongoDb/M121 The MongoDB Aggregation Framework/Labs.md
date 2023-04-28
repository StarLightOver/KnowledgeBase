# Lesson 1

### lab 1

```javascript
var pipeline = [
    {
        $match: {
            "imdb.rating": {$gte: 7},
            genres: {$nin: ["Crime", "Horror"]},
            rated: {$in: ["PG", "G"]},
            languages: {$all: ["English", "Japanese"]}
        }
    }
]

db.movies_sample.aggregate(pipeline)
```

### lab 2

```javascript
var pipeline = [
    {
        $match: {
            "imdb.rating": {$gte: 7},
            genres: {$nin: ["Crime", "Horror"]},
            rated: {$in: ["PG", "G"]},
            languages: {$all: ["English", "Japanese"]}
        }
    },
    {
        $project: {
            "_id": 0,
            "title": 1,
            "rated": 1,
        }
    }
]

db.movies_sample.aggregate(pipeline)
```

### lab 3

```javascript
var pipeline = [
    {
        $match: {
            title: {$type: "string"}
        }
    },
    {
        $project: {
            title: {$split: ["$title", " "]},
            _id: 0
        }
    },
    {
        $match: {
            "title": {$size: 1}
        }
    }
]

db.movies_sample.aggregate(pipeline)
```

### lab 4

$setIntersection - https://www.mongodb.com/docs/manual/reference/operator/aggregation/setIntersection/

```javascript
var pipeline = [
    {
        $match: {
            cast: { $elemMatch: { $exists: true } },
            directors: { $elemMatch: { $exists: true } },
            writers: { $elemMatch: { $exists: true } }
        }
    },
    {
        $project: {
            _id: 0,
            cast: 1,
            directors: 1,
            writers: {
                $map: {
                    input: "$writers",
                    as: "writer",
                    in: {
                        $arrayElemAt: [
                            { $split: ["$$writer", " ("] }, 0
                        ]
                    }
                }
            }
        }
    },
    {
        $project: {
            labor_of_love: {
                $gt: [ { $size: { $setIntersection: ["$cast", "$directors", "$writers"] } }, 0 ]
            }
        }
    },
    {
        $match: { labor_of_love: true }
    },
    {
        $count: "labors of love"
    }
]

db.movies_sample.aggregate(pipeline)
```

# Lesson 2

### lab 1

```javascript
favorites = [
    "Sandra Bullock",
    "Tom Hanks",
    "Julia Roberts",
    "Kevin Spacey",
    "George Clooney"]

var pipeline = [
  {
    $match: {
      "tomatoes.viewer.rating": { $gte: 3 },
      countries: "USA",
      cast: {
        $in: favorites
      }
    }
  },
 {
    $project: {
      _id: 0,
      title: 1,
      "tomatoes.viewer.rating": 1,
      cast: 1,
      num_favs: {
        $size: {
          $setIntersection: [ favorites, "$cast" ]
        }
      }
    }
  },
  {
    $sort: { num_favs: -1, "tomatoes.viewer.rating": -1, title: -1 }
  },
  {
    $skip: 24
  },
  {
    $limit: 1
  }
]
```

### lab 2

```javascript
var x_max = 1521105
var x_min = 5
var min = 1
var max = 9

var pipeline = [
    {
        $match: {
            year: { $gte: 1990 },
            languages: { $in: ["English"] },
            "imdb.votes": { $gte: 1 },
            "imdb.rating": { $gte: 1 }
        }
    },
    {
        $project: {
            _id: 0,
            title: 1,
            "imdb.rating": 1,
            "imdb.votes": 1,
            normalized_rating: {
                $avg: [
                    "$imdb.rating",
                    {
                        $add: [
                            min,
                            {
                                $multiply: [
                                    max,
                                    {
                                        $divide: [
                                            { $subtract: ["$imdb.votes", x_min] },
                                            { $subtract: [x_max, x_min] }
                                        ]
                                    }
                                ]
                            }
                        ]
                    }
                ]
            }
        }
    },
    { $sort: { normalized_rating: -1 } },
    { $limit: 1 }
]
```

# Lesson 3

### lab 1

```javascript
var pipeline = [
    {
        $match: {
            "awards.text": /Won \d{1,2} Oscars?/
        }
    },
    {
        $group: {
            _id: null,
            highest_rating: {$max: "$imdb.rating"},
            lowest_rating: {$min: "$imdb.rating"},
            average_rating: {$avg: "$imdb.rating"},
            deviation: {$stdDevSamp: "$imdb.rating"}
        }
    }
]
```

### lab 2

```javascript
```javascript
var pipeline = [
    {
        $match: {
            languages: "English"
        }
    },
    {
        $project: { _id: 0, cast: 1, "imdb.rating": 1 }
    },
    {
        $unwind: "$cast"
    },
    {
        $group: {
            _id: "$cast",
            numFilms: { $sum: 1 },
            average: { $avg: "$imdb.rating" }
        }
    },
    {
        $project: {
            numFilms: 1,
            average: {
                $divide: [{ $trunc: { $multiply: ["$average", 10] } }, 10]
            }
        }
    },
    {
        $sort: { numFilms: -1 }
    },
    {
        $limit: 1
    }
]

db.movies_sample.aggregate(pipeline)
```
```

### lab 3

```javascript

```

### lab 3

```javascript
var pipeline = [
  {
    $match: {
      airplane: /747|380/
    }
  },
  {
    $lookup: {
      from: "air_alliances",
      foreignField: "airlines",
      localField: "airline.name",
      as: "alliance"
    }
  },
  {
    $unwind: "$alliance"
  },
  {
    $group: {
      _id: "$alliance.name",
      count: { $sum: 1 }
    }
  },
  {
    $sort: { count: -1 }
  }
]

db.air_routes.aggregate(pipeline)
```

### lab 4

```javascript
var pipeline = [
  {
    $match: { name: "OneWorld" }
  }, {
    $graphLookup: {
      startWith: "$airlines",
      from: "air_airlines",
      connectFromField: "name",
      connectToField: "name",
      as: "airlines",
      maxDepth: 0,
      restrictSearchWithMatch: {
        country: { $in: ["Germany", "Spain", "Canada"] }
      }
    }
  }, {
    $graphLookup: {
      startWith: "$airlines.base",
      from: "air_routes",
      connectFromField: "dst_airport",
      connectToField: "src_airport",
      as: "connections",
      maxDepth: 1
    }
  }, {
    $project: {
      validAirlines: "$airlines.name",
      "connections.dst_airport": 1,
      "connections.airline.name": 1
    }
  },
  { $unwind: "$connections" },
  {
    $project: {
      isValid: { $in: ["$connections.airline.name", "$validAirlines"] },
      "connections.dst_airport": 1
    }
  },
  { $match: { isValid: true } },
  { $group: { _id: "$connections.dst_airport" } },
  { $sort: { _id: 1 } }
]

db.air_routes.aggregate(pipeline)
```

# Lesson 4

### lab 1

```javascript
var pipeline = [
  {
  $match: {
    'tomatoes.critic.rating': { $gte: 0 },
    'imdb.rating': { $gte: 0 }
  }
  }, {
  $project: {
    _id: 0,
    tomatoes: 1,
    imdb: 1,
    title: 1
  }
  }, {
  $facet: {
    top_tomatoes_critic: [
      {
        $sort: { 'tomatoes.critic.rating': -1, title: 1 }
      }, {
        $limit: 50
      }, {
        $project: { title: 1 }
      }
    ],
    top_imdb: [
      {
        $sort: { 'imdb.rating': -1, title: 1 }
      }, {
        $limit: 50
      }, {
        $project: { title: 1 }
      }
    ]
  }
  }, {
    $project: {
      movies_in_both: {
        $setIntersection: [ '$top_tomatoes_critic', '$top_imdb' ]
      }
    }
  }
]

db.movies_sample.aggregate(pipeline).itcount()
```

# Lesson 5

### lab 1

```javascript

```

### lab 2

```javascript

```

### lab 3

```javascript

```

### lab 4

```javascript

```

# Lesson 6

### lab 1

```javascript

```

### lab 2

```javascript

```

### lab 3

```javascript

```

### lab 4

```javascript

```