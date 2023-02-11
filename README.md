# MongoDBForMe

## Normal DB vs MongoDB

| SQL | MongoDB |
|----------|----------|
| Table | Collection |
| Record | Row |
|Column | Field|


## Starting MongoDB
Data can be viewed/queried using Studio 3T
```javascript
/Users/<userfolder>/mongodb/bin/mongod.exe --dbpath=/Users/<userfolder>/mongodb-data
```

## Connect to DB
```javascript
MongoClient.connect(connectionUrl, { useNewUrlParser: true }, (error, client) => {
    if (error) {
        return console.log("unable to connect to DB")
    }
    console.log("Connected Successfully!")
    const db = client.db(databaseName)
```
## Insert a document
```javascript
db.collection('user').insertOne(
        {
            name: "Ravi",
            age: 37
        })
```
## Object Id 
Id in mongodb is stored as function Object(). The reason for this is because the size is reduced by half when compared to storing as a string.
```javascript
const id = new mongodb.ObjectId
//12 byte binary buffer
console.log(id.id)
//24 character string representaton 
console.log(id.toHexString())
```
## Query for one Document
```javascript
 //Querying the document using id
    db.collection('users').findOne({ _id: new ObjectId("6386bc8786ea64fef3d6e0fd") }, (error, user) => {
        if (error) {
            return console.log('unable to fetch data')
        }
        console.log(user)
    })

    //Querying the document using name and age
    db.collection('users').findOne({ name: 'Ravi', age: 37 }, (error, user) => {
        if (error) {
            return console.log('unable to fetch data')
        }
        console.log(user)
    })


    //Querying the document using name
    db.collection('users').findOne({ name: 'Ravi' }, (error, user) => {
        if (error) {
            return console.log('unable to fetch data')
        }
        console.log(user)
    })

    //Querying multiple documents using name. Returns a cursor unlike find one
    https://www.mongodb.com/docs/drivers/node/current/usage-examples/find/

     //Querying multiple documents. This returns a cursor unlike find one
    db.collection('users').find({ age: 37 }).toArray((error,users)=>{
        console.log(users)
    })

    //count of rows. Cursor has many methods that we can explore in documentation
    db.collection('users').find({ age: 37 }).count((error,count)=>{
        console.log(count)
    })
```
## Mongoose
It is a nodejs module used for adding constraints to the data, for data modelling and many others.

## Mongoose Model
```javascript
const Tasks = mongoose.model('Tasks',{
    description:{
        type: String
    },
    completed:{
        type: Boolean
    }
})

const todo = new Tasks({
    description: 'Complete Nodejs',
    completed: false
})
```
## Data Validation & Data Sanitization
Data Validation is for intance putting a condition like age > 20
Data Sanitization is for instance trimming empty spaces around the name
Reference : https://mongoosejs.com/docs/validation.html
```javascript
const User = mongoose.model('User',{
    name: {
        type: String,
        required: true
    },
    email:{
        type: String,
        required: true,
        validate(value){
            //node module validator
           if(!validator.isEmail(value)){
                throw new Error(value + " is not a valid email address")
           }    
        }
        
    },
    age: {
        type: Number,
        required: true,
        validate(value){
            //user defined validator
            if(value<0){
                throw new Error("Age cannot be less than zero")
            }
        }
    }
})

//instantiating the class defined above
const me = new User({
    name: 'Rajesh',
    email: "test@test.com",
    age: 37
   
})
//.save returns a promise
me.save().then(()=>{
    console.log(me)
}).catch((error)=>{
    console.log('Error:', error)
})
```
## Automatic capturing of request json
```javascript
//to automatically parse the request body and make it available in req.
app.use(express.json())

app.post('/users',(req,res)=>{

    //req.body is the request body parsed automatically by line 9
    const user = new User(req.body)
    user.save().then((data)=>{
        res.status(201)
        res.send(data)
    }).catch((e)=>{
        res.status(400)
        res.send(e)
    })
})
```

## Mongoose Queries
https://mongoosejs.com/docs/queries.html

## Promise Chaining
```javascript
const add =(a,b)=>{
   return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve(a+b)
        },2000)
   })
}

/* //nested function calls
add(1,2).then((sum)=>{
    console.log(sum)
    add(sum,2).then((sum2)=>{
        console.log(sum2)
    }).catch((e)=>{
        console.log(e)
    })
}).catch((e)=>{
    console.log(e)
}) */

