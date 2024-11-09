# In-depth exploration of MongoDB queries

Here we explore some of the queries and learn the use case of them. We use nosql booster command shell for this. stuido 3t, command prompt or mongosh command shell can also be used for it.

## Insert 
we can use `insert` command for inserting data into database collection.

```Powershell
db.test.insert({name: "Mr x"}) //insert command is deprecated now
```
`insert` command is deprecated now. So, we have to use `insertOne` & `insertMany` command for adding one & multiple data in the database collection.

```Powershell
db.test.insertOne({name:"Mr x"}) //adding one data
db.test.insertMany([
    {name:"Mr x"},
    {name: "Mr z"}
    ]) //adding multiple datas

```
If we want to add multiple data, we have send it as array of object.