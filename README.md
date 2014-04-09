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

