# Ways to use Mongodb Compass
- show dbs
- use Practice ["here Practice is a database name"]
- db.createCollection("Test-1") ["this is about creating a cluster of database"]
- db.getCollection("Test-1").insertOne({name:"Sazid"})
- db.getCollection("Test-1").find()

# Using NoSqlBooster GUI with different operators and methods
### Understanding of Insert, insertOne, insertMany, find, findOne, field filtering and project 
- db.waaa.insert({name:"tonmoy"}) this will insert this data but this is deprecated so its better to use insertOne
- db.waaa.insertOne({name:"tonmoya"})
- db.waaa.insertMany([
    { name: "Sara" },
    { name: "Para" }
])
- db.waaa.find() this will find all the data 
- db.waaa.find({gender: "Male"}) this will show all the Male gender and it will give all the data and parameters available
- db.waaa.find({gender: "Male"}, {gender :1 }) this will show  only the gender field of the data and this is called fill filtering
- More Fill Filtering when we want too show multiple field of data db.waaa.find({gender: "Male"}, {gender :1, name:1, age:1, email:1})
- Project is similar to fill filtering except one problem. The problem is project nly works with find. if we use findOne it will not work db.waaa.find({gender:"Female"}).project({name:1, gender:1}) 

### Understanding of $eq, $neq, $gt, $lt, $gte, $lte operator
- Structure of defining operator {field:  {$eq : value}}
- db.waaa.find({gender : {$eq : "Female"}}, {gender:1, name:1})  ---- $neq is opposite to the $eq
- db.waaa.find({age : {$gt : 18}})  ---- $lt is opposite to $gt
- db.waaa.find({age: {$gte : 18}}).sort({age:1}) --- $lte is opposite to $gte

### Understanding of $in, $nin Operator and Implicit And Condition
- Implicit and means adding extra condition using ","
- db.waaa.find({age : {$gte :18, $lte : 30}}, {age:1}).sort({age:1}) this will give the data whose age is greater than equal too 18 and less than equal to 30. this two condition is merged using implicit and 
 - We can match multiple filed using implicit add as well db.waaa.find({
    gender:"Female",
    age: {$gte:18, $lte:30}
},{age:1, gender:1}).sort({age:1})
- $in operator selects the documents where the value of a field is equal to any value in the specific array
- Structure of $in and $nin {field: {$in : [value1, value2, value3]}}
- db.waaa.find({
    gender: "Female",
    age: { $in: [18, 20, 22, 24, 28, 30] }
},
    { age: 1, gender: 1 }
).sort({ age: 1 })

- db.waaa.find({
    gender: "Female",
    age: { $nin: [18, 20, 22, 24, 28, 30]},
    interests : {$in :["Cooking","Gaming"]}
},
    { age: 1, gender: 1, interests:1 }
).sort({ age: 1 })

### Understanding of $and, $or and implicit vs explicit
- using implicit db.waaa.find({age: {$ne :15, $lte:30}})
- using implicit db.waaa.find({age: {$ne :15, $lte:30}}).project({age:1})
- we will use explicit and when conditions are more nested db.waaa.find({
    $and: [
        {gender:"Female"},
        {age:{$ne :15}},
        {age:{$lte :30}}
    ]
}).project({age:1, gender:1}).sort({age:1})

- $or method db.waaa.find({
    $or: [
    {interests: "Cooking"},
    {interests : "Gaming"}
    ]
}).project({interests :1})

- if there is need to access mre nested data filed db.waaa.find({
    $or: [
        {"skills.name" : "JAVASCRIPT"},
        {"skills.name" : "PYTHON"}
    ]
}).project({skills :1})   --- as this is searching in same field so we can search using $in as well db.waaa.find({ "skills.name" : {$in : ["JAVASCRIPT", "PYTHON"]}}).project({"skills.name":1})