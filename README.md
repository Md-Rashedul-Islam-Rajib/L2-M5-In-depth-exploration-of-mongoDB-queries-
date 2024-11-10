# In-depth exploration of MongoDB queries

Here we explore some of the queries and learn the use case of them. We use nosql booster command shell for this. stuido 3t, command prompt or mongosh command shell can also be used for it. we use database named `test` here.

## Insert 
we can use `insert` command for inserting data into database collection.

```ps
db.test.insert({name: "Mr x"}) //insert command is deprecated now
```
`insert` command is deprecated now. So, we have to use `insertOne` & `insertMany` command for adding one & multiple data in the database collection.

```ps
db.test.insertOne({name:"Mr x"}) //adding one data
db.test.insertMany([
    {name:"Mr x"},
    {name: "Mr z"}
    ]) //adding multiple datas

```
If we want to add multiple data, we have send it as array of object. 


## Find

we can use `findOne` command to find a existing data from database collection

```ps
db.test.findOne({gender: "male"})
```

If we need to find multiple data, the we can use `find` command
```ps
db.test.find({age: 25})
```


## Field filtering
sometime we need to search one or multiple data specific propertys or fields. We can use field filtering for this.

suppose we have a lot data like this following structure in the database
```typescript
const persons = [
    {
        name: "Mr x",
        age: 20,
        email: x@gmail.com

    },{
        name: "Mr y",
        age: 20,
        email: y@gmail.com
    }

]
```
we want to find all the data with having `age: 20` but only show the email field. so we have to write the query like this

```ps
db.test.find({age: 20}, {email: 1})
```
here the first object is the search query and the search object is the field we want to return with each data.

there is another way to apply field filtering method with the `project` command

```ps
db.test.find({age: 20}).project({email:1})
```

the result will be the same with the both ways but `project` command won't work with `findOne` command but the other works fine



## $eq Operator

eq mean equal here . Sometime we need find a data which equals to a static value. for this we use `$eq` operator

```ps
db.test.find({age: {$eq : 20}})
```

## $ne Operator

ne mean not equal here . this operator works exact opposite from `$eq`.return only those data what doesn't match the condition.

```ps
db.test.find({age: {$ne : 20}})
```

## $gt Operator

gt mean greater than here . when we need to find a or multiple data which is greater than a static value. for this we use `$gt` operator

```ps
db.test.find({age: {$gt : 18}})
```

## $gte Operator

gte mean greater than and equal here . it returns all of those data which greater than and equals to a static value.

```ps
db.test.find({age: {$gte : 18}})
```


## $lt Operator

lt means less than here .its the complete opposite of `$gt` operator. When we need find a or multiple data which less than a static value.

```ps
db.test.find({age: {$lt : 29}})
```

## $lte Operator

lte mean less than and equal here . it returns all of those data which less than and equals to a static value.

```ps
db.test.find({age: {$lte : 20}})
```

## $in Operator

`$in` operator select the data when a value of a field equals any value of a selected array

```ps
db.test.find({interests : { $in : ["Traveling", "Reading"]}})
db.test.find({age : { $in : [18,20]}})
```
`$in` operator only accepts array as parameter.


## $nin Operator

`$nin` operator is the complete opposite of `$in` operator. It select the data when a value of a field not equals any value of a selected array

```ps
db.test.find({interests : { $nin : ["Cooking", "Dancing"]}})
db.test.find({age : { $nin : [25,27]}})

```

## Implicit and `,` operator

Sometime we need to find the data with the conditions from multiple fields or filtering with multiple conditions. That's why we need a operator. Here comes the implicit and `,` operator

```ps
db.test.find({gender: "Female", age: 25, occupation: "Teacher"})


db.test.find({gender: "Female", age:{$in: [25,27]}, occupation: {$eq : "Teacher"}})


db.test.find({age:{$lte: 25, $gte : 18}})
```

## $and Operator (Explicit)

We can also use `$and` operator for connecting multiple conditions for filtering data


```ps
db.test.find({$and : [
    {gender : "Female"},
    {age : 25},
    {occupation : "Teacher"}
]})


db.test.find({$and : [
    {gender : "Female"},
    {age : {$in : [25,27]}},
    {occupation : { $eq : "Teacher"}}
]})


db.test.find({$and : [
    {age: {$lte : 25}},
    {age: {$gte : 16}}
]})
```

## $or Operator

If we need to select data for satisfies one condition from multiple conditions then we can use `$or` operator

```ps
db.test.find({$or : [
    {gender : "Female"},
    {age : 25},
    {occupation : "Teacher"}
]})


db.test.find({$or : [
    {gender : "Female"},
    {age : {$in : [25,27]}},
    {occupation : { $eq : "Teacher"}}
]})


db.test.find({$or : [
    {age: {$lte : 25}},
    {age: {$gte : 16}}
]})
```

## $not Operator

`$not` Operator select the data what doesn't match the condition

```ps
db.test.find({age : {$not : { $ gt : 25}}})
```


## $not Operator

`$nor` Operator select datas when it failed to match with all the queries in the selected array


```ps
db.test.find({$nor : [
{age: {$gt : 25}},
{gender : "Male"}
]})
```

## $exists Operator

`$exists` operator match the data based on a selected field contain or not on that document.

```ps
db.test.find({occupation : {$exists : true}})
```

`$exists` operator supports only boolean value.