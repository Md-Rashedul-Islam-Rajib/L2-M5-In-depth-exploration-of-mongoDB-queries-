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



