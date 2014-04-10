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

Write concerns:

w=0 - No concern but gets a network acknowledge

w=1 - Waits for an error/ack from primary (get last error). Successful write to mongod.
      Data written to mongod in memory. Don't know if it made it to journal or disk/etc. It might only exist in memory. If machine loses power at the moment data won't be durable.
      Default write concern.

j=1 - Sets the journal true and wait for journal to be commmited to disk.
      This is slow. Not generally recommended because there is a disk access in middle
      of writes. It is safer but there is a performance hit.

w>1 - How many mongod's the wright has to be successful for the replica sets. Write to
      memory that is. (w=majority also works)

Read preferences:

5 Modes:
- primary (only) - Default
- primaryPreferred
- secondary
- secondaryPreferred
- Nearest
 When more than one node is possible, closest node is used for reads (all modes but primary)

Tagged Read Preference:

Custom read preferences
Control where you read from by (node) tags:
	- EG {"disk":"ssd", "use":"reporting"}
Use in conjunction with standard read preferences
        - Except primary

Why replica sets good?

Maintenance and Upgrade

No downtime.

Rolling upgrade/maintenance
	- Start with secondary
	- Primary last

var conf = rs.conf()
edit conf <-- if
rs.reconfig(conf, {"force": true})

a:PRIMARY> ls() <-- lists data files in current working directory

a:PRIMARY> load ("gen_data.js") <-- runs a script within mongo

rs.slaveOk() <-- if you want to read from secondary repl server. 
                 Needed if you want to do things on slave. Should only read.

rs.stepDown() <-- if on primary it will become secondary.

rs.isMaster() <-- shows master
set0:SECONDARY> rs.isMaster()
{

	"setName" : "set0",

	"setVersion" : 126531,

	"ismaster" : false,

	"secondary" : true,

	"hosts" : [

		"localhost:30003",

			"localhost:30002",

				"localhost:30000"

				],

				"primary" : "localhost:30002",

				"me" : "localhost:30003",

				"maxBsonObjectSize" : 16777216,

				"maxMessageSizeBytes" : 48000000,

				"maxWriteBatchSize" : 1000,

				"localTime" : ISODate("2014-04-10T22:04:41.854Z"),

				"maxWireVersion" : 2,

				"minWireVersion" : 0,

				"ok" : 1
}


