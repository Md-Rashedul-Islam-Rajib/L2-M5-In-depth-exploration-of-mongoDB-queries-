# In-depth exploration of MongoDB queries

Here we explore some of the queries and learn the use case of them. We use nosql booster command shell for this. stuido 3t, command prompt or mongosh command shell can also be used for it. we use database named `test` here.

## Insert

we can use `insert` command for inserting data into database collection.

```javascript
db.test.insert({name: "Mr x"}) //insert command is deprecated now
```

`insert` command is deprecated now. So, we have to use `insertOne` & `insertMany` command for adding one & multiple data in the database collection.

```javascript
db.test.insertOne({name:"Mr x"}) //adding one data
db.test.insertMany([
    {name:"Mr x"},
    {name: "Mr z"}
    ]) //adding multiple datas

```

If we want to add multiple data, we have send it as array of object.

## Find

we can use `findOne` command to find a existing data from database collection

```javascript
db.test.findOne({gender: "male"})
```

If we need to find multiple data, the we can use `find` command

```javascript
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

```javascript
db.test.find({age: 20}, {email: 1})
```

here the first object is the search query and the search object is the field we want to return with each data.

there is another way to apply field filtering method with the `project` command

```javascript
db.test.find({age: 20}).project({email:1})
```

the result will be the same with the both ways but `project` command won't work with `findOne` command but the other works fine

## $eq Operator

eq mean equal here . Sometime we need find a data which equals to a static value. for this we use `$eq` operator

```javascript
db.test.find({age: {$eq : 20}})
```

## $ne Operator

ne mean not equal here . this operator works exact opposite from `$eq`.return only those data what doesn't match the condition.

```javascript
db.test.find({age: {$ne : 20}})
```

## $gt Operator

gt mean greater than here . when we need to find a or multiple data which is greater than a static value. for this we use `$gt` operator

```javascript
db.test.find({age: {$gt : 18}})
```

## $gte Operator

gte mean greater than and equal here . it returns all of those data which greater than and equals to a static value.

```javascript
db.test.find({age: {$gte : 18}})
```

## $lt Operator

lt means less than here .its the complete opposite of `$gt` operator. When we need find a or multiple data which less than a static value.

```javascript
db.test.find({age: {$lt : 29}})
```

## $lte Operator

lte mean less than and equal here . it returns all of those data which less than and equals to a static value.

```javascript
db.test.find({age: {$lte : 20}})
```

## $in Operator

`$in` operator select the data when a value of a field equals any value of a selected array

```javascript
db.test.find({interests : { $in : ["Traveling", "Reading"]}})
db.test.find({age : { $in : [18,20]}})
```

`$in` operator only accepts array as parameter.

## $nin Operator

`$nin` operator is the complete opposite of `$in` operator. It select the data when a value of a field not equals any value of a selected array

```javascript
db.test.find({interests : { $nin : ["Cooking", "Dancing"]}})
db.test.find({age : { $nin : [25,27]}})

```

## Implicit and `,` operator

Sometime we need to find the data with the conditions from multiple fields or filtering with multiple conditions. That's why we need a operator. Here comes the implicit and `,` operator

```javascript
db.test.find({gender: "Female", age: 25, occupation: "Teacher"})


db.test.find({gender: "Female", age:{$in: [25,27]}, occupation: {$eq : "Teacher"}})


db.test.find({age:{$lte: 25, $gte : 18}})
```

## $and Operator (Explicit)

We can also use `$and` operator for connecting multiple conditions for filtering data

```javascript
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

```javascript
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

```javascript
db.test.find({age : {$not : { $ gt : 25}}})
```

## $not Operator

`$nor` Operator select datas when it failed to match with all the queries in the selected array

```javascript
db.test.find({$nor : [
{age: {$gt : 25}},
{gender : "Male"}
]})
```

## $exists Operator

`$exists` operator match the data based on a selected field contain or not on that document.

```javascript
db.test.find({occupation : {$exists : true}})
```

`$exists` operator supports only boolean value.

## $type Operator

`$type` operator select data based on selected field's value type

```javascript
db.test.find({skills : {$type : "array"}})
```

## $size Operator

`$size` operator select data based on selected field's property size

```javascript
db.test.find({skills : {$size : 3}})
```

`$size` operator works only against array.

## $all Operator

MongoDB works on array data in direct approach. Suppose, you have data structured like this

```ts
const students =
    {
        name : "Mr x",
        subjects : ["bangla","english","math"]
        },{
        name: "Mr y",
        subjects: ["english", "math", "biology"]
        }

```

now you want to select the data which has a subject value both "english" & "math". you can write the query for it like this

```javascript
db.test.find({subjects : ["english","math"]}) //this query won't works
```

but this query won't give you any data because no data hold exactly those and also not maintaining those order in that array. This is where `$all` operator will help you

```javascript
db.test.find({subjects : {$all : ["english","math"]}})
```

this will return the value what data's subjects field hold those value without looking their order.

## $elemMatch Operator

MongoDB also works on object in a direct approach like array.If a data have nested object like this

```ts
persons = {
    {
        name: "Mr x",
        skills: {
            frontend : "react",
            backend : "node",
            database : "mongodb",
            level : "intermediate"
        }
        },
    {
        name: "Mr Y",
        skills: {
            frontend : "react",
            backend : "node",
            database : "postgresql",
            level: "beginner"
        }
        },
}
```

now you want to select the data what's skill property hold the `forntend : "react"` & `backend : "node"`. you can write this

```javascript
db.test.find({skills: {
    frontend : "react",
    backend : "node"
}})  //this query won't works
```

this query won't works because no data hold skills field with only forntend & backend. now we can use `$elemMatch` for solve this problem

```javascript
db.test.find({skills : {$elemMatch : {
    frontend : "react",
    backend : "node"
}}})
```
this query select the data what skill section holds those value and ignore the other fields for match


## Update command with $set operator

if we need to update any fields from existing data we can use update command with `$set` operator.

```javascript
db.test.updateOne(
    {name : "Mr x"},  // find query 
    {
        $set : {
            age : 60
        }
    }
    )
```

## $addToSet Operator

`$set` operator has a drawback. it's always replace the existing value with new value but if we need add new value with existing value, then we have to use `$addToSet`.

```javascript
db.test.updateOne(
    {name : "Mr x"}, //find query
    {
        $addToSet : {
            interests : ['sleeping', 'gaming']
        }
    }
)
```

this will add `sleeping` & `gaming` to skills field with existing value but it can't add duplicate value.


## $push Operator

if we need to put duplicate value to a field of a data, then we need to use `$push` operator

```javascript
db.test.updateOne(
    {name : "Mr x"}, //find query
    {
        $addToSet : {
            interests : ['sleeping', 'gaming'] //this value will duplicated if they already exists in the data
        }
    }
)
```

## $unset operator

`$unset` operator deletes a field from a data


```javascript
db.test.updateOne(
    {name : "Mr x"}, //find query
    {
        $unset : {
            interests : ""; // you can use 1 insteads "" for same results
        }
    }
)
```

## $pop Operator

`$pop` operator delete the last element from an array from a field

```javascript
db.test.updateOne(
    {name : "Mr x"}, //find query
    {
        $pop : {
            skills : 1; // this will delete the last element from skill array
        }
    }
)
```

## $pull Operator

if we need to delete the a value of a field but not the field then we can use `$pull` operator


```javascript
db.test.updateOne(
    {name : "Mr x"}, //find query
    {
        $pull : {
            occupation : "Teaching"; // this will delete the value "Teaching" but keep the occupation field with empty value
        }
    }
)
```

## $pullAll Operator

`$pullAll` is similar to `$pull` operator but `$pullAll` operator can delete multiple value from data at any time


```javascript
db.test.updateOne(
    {name : "Mr x"}, //find query
    {
        $pullAll : {
            skills : ["Teaching", "Cooking"]; // this will delete the value "Teaching" and "cooking" but keep the occupation field with empty value
        }
    }
)
```

