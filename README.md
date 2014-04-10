mongodb_essentials
==================

MongoDB Essentials Training Class in San Diego, CA 4/9/14 - 4/11/14

### Day One

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

###Day Two:

Snapshot a doc if you are worried about a write changing something as you're reading.
$snapshot is the token. Doesn't come up often but be aware that it could happen.
Walks the _id and only visits each one once with $snapshot. So if the write affected the doc and moved it to another memory segment that hasn't been scanned yet it won't use it since it already used the _id.

#####Schema Design:

Invite DBA's early to be involved. Schema's tend to happen sooner in MongoDB rather than later as in most RDBMs.

Memory, Disk, I/O, indexes, etc. All factors in what the schema will look like.

Whether to store embedded data positionally (with arrays) or by named fields (with nested docs). This is often a matter of taste, but sometimes relates to what can be queried/indexed efficiently.

Arrays can get large and costly.

When data immutable (or seldomly mutated), it is easier to separate it out into another document.

Got into groups and did a schema exercise to mimic an ecommerce site for products, reviews, rating, price, price history, categories, and the relationships between them.

####Replica Sets

Why replication?

Availability and durability ... of the data. Not designed for scaling reads. Use sharding for scaling!!

