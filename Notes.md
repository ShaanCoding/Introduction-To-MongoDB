# Introduction to MongoDB

## Introduction

- NoSQL Database (Not only SQL)
- NoSQL Data is stored in collection of documents
- Documents are similar to JS objects (in BSON (with additional datatypes))
- Incredibly popular in JS community it is the M in MERN and etc stack

- Got replication & sharding, great horiziontal scaling. Performance & fast. It's like a javascript object, but you don't have to map out entire data structure like a relational database

```
show dbs

use test1

show collections

db.dropDatabase()

use test1 //wont show up unless we have collections

to check what db we're in do `db`

db.createCollection('posts')
```

```mongodb
<!-- Adding an item -->

db.posts.insert({
    title: "Post One",
    body: "Body of post one",
    category: "News",
    likes: 4,
    tags: ["news", "events"],
    user: {
        name: "Shaan Khan",
        status: "Author"
    },
    date: Date()
})

```

If we want to insert > 1 at a time, we can use insertMany

```mongodb
db.posts.insertMany([{
    title: "Post One",
    body: "Body of post one",
    category: "News",
    likes: 4,
    tags: ["news", "events"],
    user: {
        name: "Shaan Khan",
        status: "Author"
    },
    date: Date()
},
{
    title: "Post One",
    body: "Body of post one",
    category: "News",
    likes: 4,
    tags: ["news", "events"],
    user: {
        name: "Shaan Khan",
        status: "Author"
    },
    date: Date()
},
{
    title: "Post One",
    body: "Body of post one",
    category: "News",
    likes: 4,
    tags: ["news", "events"],
    user: {
        name: "Shaan Khan",
        status: "Author"
    },
    date: Date()
}])
```

If we want to query data

```mongodb

db.posts.find()

This is hard to read so we can do

db.posts.find().pretty()
```

Narrow it down to only find news categories

```mongodb

db.post.find({category: "News"}).pretty()

```

How to sort

```mongodb
db.posts.find().sort({title: 1}).pretty()

1 for ascending
-1 for descending

```

To find how many searches found

```mongodb
db.post.find({category: "News"}).count()
```

If we want to limit records

```mongodb
db.posts.find().limit(2)
```

If we want to use a foreach like printing each post out

```mongodb

db.posts.find().forEach(function(doc) {
    print('Blog Post: ' + doc.title)
})
```

Find one

```mongodb
db.posts.findOne({category: "News"})
```

Updating a post

```mongodb
db.posts.update()

<!-- 1st way -->
<!-- Replace the entire thing -->
<!-- Should use the ID since duplicate titles -->
db.posts.update({title: "Post Two"},
{
    title: "Post Two",
    body: "Revised post two body",
    date: Date()
},
{
    upsert: true
    <!-- If it isn't found (this will create it), this is options -->
}
)
```

Alternatively if you want to replace one or two fields with stuff already there, you want to use the SET operator

```mongodb
db.posts.update({title: "Post Two"},
{
    $set: {
        body: "This is a revised post 2",
        category: "Technology"
    }
},
{
    upsert: true
})
```

How to update numbers += 2

```
db.posts.update({title: "Post One"},
{
    $inc {
        likes: 2
    }
})
```

Rename a field i.e likes to views

```mongodb
db.posts.update({title: "Post One"},
{
    $rename: { likes: "views" }
})
```

How to delete a post

```mongodb
db.posts.remove({title: "Post Four"})
<!-- All gone now -->
```

Subdocuments

- In a relational database i.e blogposts and comments, we would need a seperate table a foreign key which links to a primary key linking it
- With mongodb we could have a subdocument that does this instead which makes it easier in many cases

```mongodb
db.posts.update({title: "Post One"},
{
    $set: {
        comments: [
            {
                user: "Mary Williams",
                body: "Comment One",
                date: Date()
            },
            {
                user: "John Doe",
                body: "Comment Two",
                date: Date()
            }
        ]
    }
})

```

Finding a post with comments w/ mary williams. We can use an operator called elemMatch

```sql
db.posts.find({
    comments: {
        $elemMatch: {
            user: "Mary Williams"
        }
    }
})
```

Adding indexes

```sql
db.posts.createIndex({title: "Text"})
```

To search we can then do

```sql
db.posts.find({
    $text: {
        $search: "\"Post 0\""
    }
}).pretty()
```

Doing greater than and less than

```sql
db.posts.update({
    title: "Post Two"
},
{
    $set {
        views: 10
    }
})
```

Greater than or less than

```sql
db.posts.find({
    views: { $gt: 3}
}).pretty()
```

for greater than or equal do gte, for less than or equal lt, or lte
