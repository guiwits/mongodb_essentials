mongodb_essentials
==================

MongoDB Essentials Training Class in San Diego, CA 4/9/14 - 4/11/14

Instructor: Norman Graham

example queries:

db.parents.find ({ "hobbies" : { $elemMatch : { "name" : "Skateboarding", "skill_level" : 1 } } })


db.places.find ({ "tags" : { $all : ["Springfield", "Bar"] }})


db.places.find ({ "tags" : { $size : 3 }}

db.places.find ({ "tags" : { $slice : 2 }})

db.places.find ({}, { "tags" : { $slice : 2 }}) <-- second doc is projection

db.places.find ({"tags.1", "Beer"})

update examples:

db.people.update ({"name" : "Lisa"}, {$set : {"instrument" : "Saxophone"}})

^^ will update one

db.people.update ({"name" : "Lisa"}, {$set : {"instrument" : "Saxophone"}}, { multi : true })

^^ will update multi

db.people.update ({"name" : {$in : ["Lisa", "Bart"]}}, {$set : {"city" : "Springfield"}}, { multi : true })

db.people.update ({"name" : "Norm"}, {$set : {"city" : "Springfield"}}, { upsert: true })

db.people.update ({"name" : "Lisa"}, {$set : {"friends":10}})

db.people.update ({"name" : "Lisa"}, {$inc : {"friends":1}})

more update examples .... 

db.schools.update({"staff.firstname" : "Edna"}, {$set : {"staff.$.position" : "5th grade teacher"}}) <-- $ is special character that represents element in array that was matched.

db.people.update ({"name":"Bart"}, {$rename : {"age" : "my_new_age" }})

db.places.update ({"name":"Quickie Mart"}, {$push: {tags : ["milk", "eggs"]}})

db.places.update ({"name":"Quickie Mart"}, {$pu: {tags : ["milk"]})}

$addToSet operator example. Adds item but only if it doesn't exist.

db.name.remove({<query>}) - Removes document

db.name.ensureIndex({name : 1}) 1 ascending, -1 descending

Compound indexes:

db.people.ensureIndex({name:1, age:1)

Geospatial indexes:

db.locations.ensureIndex ({lat_long : "2d"})

db.locations.find ({"lat_long" : {$near : [-122.143019, 37.441883]}})

db.locations.find({"lat_long" : {"$withhin" : {"$center" : [[-122, 37], 5]}}})

My exercies from book:

db.scores.find ({$or : [ {"kind" : "quiz"}, {"kind" : "exam"} ] })

db.scores.find ({ "kind" : { $in : ["quiz", "exam"]}})