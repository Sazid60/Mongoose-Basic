# Ways to use Mongodb Compass
- show dbs
- use Practice ["here Practice is a database name"]
- db.createCollection("Test-1") ["this is about creating a cluster of database"]
- db.getCollection("Test-1").insertOne({name:"Sazid"})
- db.getCollection("Test-1").find()

# Using NoSqlBooster GUI with different operators and methods
### Understanding of Insert, insertOne, insertMany, find, findOne, field filtering and project 
- this will insert this data but this is deprecated so its better to use insertOne
  ```javascript
  db.waaa.insert({name:"tonmoy"})
  ```
  ```javascript
  db.waaa.insertOne({name:"tonmoya"})
  ```
  ```javascript
  db.waaa.insertMany([
    { name: "Sara" },
    { name: "Para" }])
    ```
- db.waaa.find() this will find all the data 
- this will show all the Male gender and it will give all the data and parameters available
  ```javascript 
  db.waaa.find({gender: "Male"})
  ```
- this will show  only the gender field of the data and this is called fill filtering
 ```javascript
 db.waaa.find({gender: "Male"}, {gender :1 })
 ```
- More Fill Filtering when we want too show multiple field of data 
  ```javascript
  db.waaa.find({gender: "Male"}, {gender :1, name:1, age:1, email:1})
  ```
- Project is similar to fill filtering except one problem. The problem is project nly works with find. if we use findOne it will not work 
  ```javascript
  db.waaa.find({gender:"Female"}).project({name:1, gender:1})
  ``` 

### Understanding of $eq, $neq, $gt, $lt, $gte, $lte operator
- Structure of defining operator 
  ```javascript
  {field:  {$eq : value}}
  ```
- $neq is opposite to the $eq
  ```javascript
  db.waaa.find({gender : {$eq : "Female"}}, {gender:1, name:1})
  ```
- $lt is opposite to $gt
```javascript
db.waaa.find({age : {$gt : 18}})  
```
- $lte is opposite to $gte
  ```javascript
  db.waaa.find({age: {$gte : 18}}).sort({age:1}) 
  ```

### Understanding of $in, $nin Operator and Implicit And Condition
- Implicit and means adding extra condition using ","
- this will give the data whose age is greater than equal too 18 and less than equal to 30. this two condition is merged using implicit and
```javascript
db.waaa.find({age : {$gte :18, $lte : 30}}, {age:1}).sort({age:1})
```  
- We can match multiple filed using implicit add as well 
```javascript
db.waaa.find({
    gender:"Female",
    age: {$gte:18, $lte:30}
},{age:1, gender:1}).sort({age:1})
```  
- $in operator selects the documents where the value of a field is equal to any value in the specific array
- Structure of $in and $nin 
```javascript
{field: {$in : [value1, value2, value3]}}
```
  
  ```javascript
  db.waaa.find({
    gender: "Female",
    age: { $in: [18, 20, 22, 24, 28, 30] }},{ age: 1, gender: 1 }).sort({ age: 1 })
  ```
  ```javascript
  db.waaa.find({
    gender: "Female",
    age: { $nin: [18, 20, 22, 24, 28, 30]},
    interests : {$in :["Cooking","Gaming"]}},{ age: 1, gender: 1, interests:1 }).sort({ age: 1 })
  ```

### Understanding of $and, $or and implicit vs explicit
- using implicit 
  ```javascript
  db.waaa.find({age: {$ne :15, $lte:30}})
  ```
- using implicit 
  ```javascript
  db.waaa.find({age: {$ne :15, $lte:30}}).project({age:1})
  ```
- we will use explicit and when conditions are more nested 
- Structure for $and and others
  ```javascript
  {$and :[{ex1},{ex2},...,{exN}]}
  ```
  ```javascript
  db.waaa.find({
    $and: [
        {gender:"Female"},
        {age:{$ne :15}},
        {age:{$lte :30}}
    ]}).project({age:1, gender:1}).sort({age:1})
  ```
- $or method 
```JAVASCRIPT
db.waaa.find({
    $or: [
    {interests: "Cooking"},
    {interests : "Gaming"}
    ]
}).project({interests :1})
```

- if there is need to access more nested data filed 
```javascript
db.waaa.find({
    $or: [
        {"skills.name" : "JAVASCRIPT"},
        {"skills.name" : "PYTHON"}
    ]
}).project({skills :1})

// as this is searching in same field so we can search using $in as well
 db.waaa.find({ "skills.name" : {$in : ["JAVASCRIPT", "PYTHON"]}}).project({"skills.name":1})
```
### Understanding of element operator, array query operator , $exists, $type, $size 
- if we want to make sure the data really exist in the documents we will use $exists. There is problem with it it can not identify null and undefined
- Structure 
  ```javascript
  {field : {$exist : <boolean>}}
  ```
- Which documents have the phone field will show here 
  ```javascript
  db.waaa.find({phone: {$exists : true}})
  ```
- as unknown is not present in the data so i will return false   
  ```javascript
  db.waaa.find({unknown: {$exists : true}})
  ```

- If we want to get specific type of data we will use $type operator
- Structure 
  ```javascript
  {field : {$type : <BASON Type>}}
  ```
  - Example
  ```javascript
  db.waaa.find({age: {$type : "string"}}).project({age:1})
  db.waaa.find({friends: {$type : "array"}}).project({friends:1})
  ```

  - If we want to figure out what if the array is empty we have to use array query operator $size. If will mention specific $size it will give us the exact sized array
  - size is used only in array
  - Structure 
  ```javascript
  db.waaa.find({friends : {$size:5}}).project({friends:1})
  ```
  - If we want to figure out any empty filed we have to use $type since the $size works only in array
  ```javascript
  db.waaa.find({company : {$type :"null"}}).project({company:1})
  ``` 

  ### Understanding of array,object and array of object query $all, $elemMtach
  - I want a specific indexed valued data we can use normal method
  ```javascript
  <!-- regular method of matching only one in array -->
  db.waaa.find({interests:"Cooking"}).project({interests:1})
  ```
  - For specific index positioned value
  ```javascript
  db.waaa.find({"interests.2":"Cooking"}).project({interests:1})
  ```
  - If we want to match exactly
  ```javascript
  db.waaa.find({interests : ["Gardening", "Gaming", "Cooking"]}).project({interests:1})  // It will follow the same ordered mentioned and give data
  ```
  - If we want to match in more flexible way without maintaining order we need to use $all operator
  - Structure
  ```javascript
  {tags : {$all : ["a","b"]}}
  ```
  - Suppose we want to match "Gardening", "Gaming", "Cooking"
  ```javascript
  db.waaa.find({interests : { $all : ["Gardening", "Gaming", "Cooking"]}}).project({interests:1})
  ```
  - For object we can do the same. this example is strict to the order. Each and every field should be present
   ```javascript
   db.waaa.find({
    skills: {
        name:"JAVASCRIPT",
        level:"Expert",
        isLearning :true
    }
   }).project({skills:1})
   ```
   - If we want to enjoy flexibility we have to use $elemMatch
  ```javascript
  <!--
   Structure :
  {field : {$elemMatch : {<query1>, <query2>,...}}}
   -->
  db.waaa.find({
    skills : {
        $elemMatch :{
            name:"JAVASCRIPT",
            level:"Expert"
        }
    }
  }).project({skills :1 })
  ```

  ### Understanding of $set, $addToSet and $push

- Structure for set method is 
  ```javascript
  {$set : {<field> : <value>,...}}

  <!-- what is the system? -->
  db.waa.UpdateOne(
    {which one we will Update},
    {what we will update},
    {options}
    )
  ```
  - for updating the age 
   ```javascript
   db.waaa.updateOne(
    {_id : ObjectId("6406ad63fc13ae5a40000065")},
    { $set: {age:20}}
    )
   ```
   - $set replaces the field with new update things. This works meaningful only for primitive data type. if its a non primitive data type its not meaningful. We can use $set for non primitive only if we want to replace intentionally.
   - For non-primitive data we will use $addToSet. This will set inside the array keeping no duplicates.
   - Structure for it is
    ```javascript
    {$addToSet : {<field> : <value>}}
    ```
   - Example of $addToSet
    ```javascript
    db.waaa.updateOne(
    {_id : ObjectId("6406ad63fc13ae5a40000065")},
    { $addToSet: {interests : "Gaming"} }
    )
    ``` 
    - If we want to add multiple things in the array we have to use $each.
    ```javascript
    db.waaa.updateOne(
    {_id : ObjectId("6406ad63fc13ae5a40000065")},
    { $addToSet: {interests : {$each : ["Sleeping", "Texting"]}}}
    )
    ``` 
    - If we want to keep the duplicates we have to use $push
    ```javascript
    <!-- Structure -->
    {$push : {<field> :<value>,...}}
    db.waaa.updateOne(
    {_id : ObjectId("6406ad63fc13ae5a40000065")},
    { $push: {interests : {$each : ["Sleeping", "Texting"]}}}
    )
    ``` 