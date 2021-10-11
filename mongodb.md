# MongoDb 

- mogodb store data in collection of documents(kinda similar to js objects)
- in mongodb we don't really have to define datastructure (lke in sql we have to specify columns datatype etc) 

## shell

- enable `mongodb` service run `mongosh`
- show dbs `show dbs`
- use db `use whateverName`
- `show collections`
- `db.dropDatabase()`
- create a new db `use acme` and if u show dbs it won't show because it has no collections 
- `db.createCollection('posts')`

```
db.posts.insertOne({
    title: 'Post One',
    body: 'Body of post one',
    category: 'news',
    date: Date(),
    likes: 4,
    tags: ['news', 'events'],
    user: {
        name: 'John Doe',
        status: 'Author'
    }
})
```

- to insert many documents just use `db.posts.insertMany([{},{}])`
- you don't have to follow some strict model insert whatever 

### find

- db.posts.find() //will return all data in posts colleciton 
- `db.posts.find({category: 'news'})` documents with this thing in them 
- for fields that have an array of values even one value is enough for a match such as `db.posts.find({tags: 'events'})` for above doc

### sort 

- db.posts.find().sort({title:1}); (we specify what is we sort with title and value of 1 means ascending and -1 means descending 

### count 

- db.posts.find().count()

### limit 

- db.posts.find().limit(2)
- db.posts.find().sort({id:-1}).limit(2) //get top two from bottom 

### you can also run stuff like forEach

- db.posts.find().forEach(function(doc){ print('Oh yeah' + doc.title) })

### findOne
-returns first match

### update 
- db.posts.update({id: 43}, {title: "oh yeah", body: "what bruh"}, {upsert: true})
- so first option is how to find it, second is the body which will completely override the found document, third are the options such as upsert true means that if not found then insert 
- to only update some fields and not remove everything else use set operator 

```
db.posts.update({title: 'post no 2'}, {
$set: {
category : "fuck"
}
})
```

#### inc operator 

- to increase some value

```
db.posts.find({title: "whatever title"}, {
    $inc: {
        likes: 2
    }
})
```

- the above query increases like values by 2 

#### rename operator 

```
db.posts.find({title: "yea ok"}, {
    $rename: {
        likes: "views"
    }
})
```

### remove

`db.posts.remove({title: "yea I don't like this one"})`

### inserting documents in documents 

```
db.posts.update({title: "yea ok let's add stuff"}, {
    $set: {
        comments: [
            {
                name: "Mary",
                comment: "Yea that's fine but where is my husband"
            },
            {
                name: "Arthur",
                comment: "Ms. Mary would you like to join me at dinner"
            }
        ]
    }
}, {upsert: true})
```

### elemMatch 

- suppose you want to find documents with comments having a comment from Mary, you could use find `db.posts.find({"comments.name": "Mary"})` (we are using quotation marks on comments.name because it will throw syntax error afterall it's a key we want matched) 
- The $elemMatch operator matches documents that contain an array field with at least one element that matches **all** the specified query criteria.
- for multiple queries we shouldn't omit elemMatch because things can go [wrong](https://docs.mongodb.com/manual/reference/operator/query/elemMatch/#std-label-elemmatch-single-query-condition)

```
db.posts.find({
    comments: {
        $elemMatch: {
            name: "Mary"
        }
    }
})
```

### createIndex 

- used to create index 
- In MongoDB, indexes are special data structures that store some information related to the documents such that it becomes easy for MongoDB to find the right data file. The indexes are ordered by the value of the field specified in the index. So, MongoDB provides a createIndex() method to create one or more indexes on collections. 

```
db.posts.createIndex({title:'text'})
```

- so basically create an index out of title and it is an text index 

### search 

- MongoDB supports query operations that perform a text search of string content. To perform text search, MongoDB uses a text index and the $text operator.
-  A collection can only have one text search index, but that index can cover multiple fields. `db.stores.createIndex( { name: "text", description: "text" } )`
-  $text will tokenize the search string using whitespace and most punctuation as delimiters, and perform a logical OR of all such tokens in the search string.
-  `db.posts.find({$text: {$search: "ok" }})`
-  to search for exact phrase wrap the query string in double quotes `db.stores.find( { $text: { $search: "\"coffee shop\"" } } )` (we escaped the quotes and therefore added them to the coffee shop phrase)

#### greater than and less than operator 

- `db.posts.find({likes : {$gt: 4}})`
- `db.posts.find({likes : {$lt: 5}})`
- `db.posts.find({likes : {$gte: 4}})` //greater than or equal to 
- `db.posts.find({likes : {$lte: 4}})`


