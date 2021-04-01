# json-server-practice

Mini project to familiarize the use of `json-server`, and practice building a basic `db.json` server and some of it's advanced functionality!

Resource used in writing this tutorial:
I am using https://github.com/typicode/json-server

Quick links

- [What is a JSON Server?](#❓❔-JSON-Server-||-json-server-||-❔❓)
- [Install json-server](#Installing-json-server)
- [Making a database](#It's-database-time!)
- [What's in a database?](#😅-What-did-I-just-do?-😅)
- [Running our server](#⚡️⚡️-IT-LIVES-⚡️⚡️!-(Taking-our-server-live!))
- [json-server interactions](#Interacting-with-our-new-DB)
- [Database complete!](#Success?)
- [Port management](#Port-management)
- [Building object relationships](#Building-object-relationships)
- [Nested data and custom routes](#Nested-data-and-custom-routes)

## ❓❔ JSON Server || json-server || ❔❓

First off, what is `json-server`? 

Well, a quick google tells us: 
>JSON Server is a Node Module that you can use to create demo REST json webservice in less than a minute using just a JSON file for sample data. 

That may seem like a good bit of gibberish right now, but we can think of it a way for us to mock a basic database server, and interact with it as if it were an external API. 

Hopefully by the end of this adventure you'll get a decent grasp of what all of that actually means and how to build one up, from scratch!

Let's not talk about what we *could* do with it, and instead see what we *can* do with it!

![let's do this](https://media2.giphy.com/media/BpGWitbFZflfSUYuZ9/200.gif)

## Installing json-server

In your terminal run (if you don't already have `json-server` installed)
```
npm install -g json-server
```

This tells npm (our Node Package Manager) to install GLOBALLY (that's the -g) the node module by the name of json-server. This should take a little bit (could be a few seconds or a couple minutes, depending on your system). Once it's done you can check if it installed by running

```
json-server -v
```

This should check which version of your json-server is currently installed. Any number (at the time of writing this README my personal version is 0.16.1) shows that you have it installed. If you don't see any numbers, try to install again and/or check through your readout on the installation to see if there were any issues.

You can also see a handy list of `json-server` options by typing
```
json-server -help
```

Once json-server is installed, let's build a database and get this party started!

![We got this](https://media0.giphy.com/media/WrmAysy2Zb2Z2qhVkS/giphy.gif)

## It's database time!

Make a file called `db.json` on the same level as our `README.md`

Making our file a `.json` type makes it a JSON file. A JSON file is a file that stores simple data structures and objects in JavaScript Object Notation (JSON) format, which in this use allows us to build items in our database in the shape a JavaScript object. For this demo we are going to build a database to build a collection of users and try to track books that they own. 

To start let's add the below text into our `db.json`
```
{
  "users": [
    { "id": 1, 
    "name": "Francis Felicity Franklin IV",
    "age" : 32 },
    { "id": 2, 
    "name": "Greg",
    "age" : 38 }
  ],
  "books": [
    { "id": 1, 
    "title": "Coding for Dummies", 
    "userId": 1 },
    { "id": 2, 
    "title": "<Yoga class=for-programmers/>", 
    "userId": 1 },
    { "id": 3, 
    "title": "Dirk Gently's Holistic Detective Agency", 
    "userId": 2 }
  ]
}
```
Make sure it's saved and there we go! We have a basic database all set up and ready! 

## 😅 What did I just do? 😅

Great question! 

We built an object that has two keys, each of which points to an array. These keys are representing the class/model of each of our types of data, and any collection you want stored in your database will need it's own unique key. For example: 
```
{
    "things":[],
    "differentThings":[],
    "singleThing":{}
}
```
Notice the naming convetion above.👆 
If your key is pointing to an array (which means it's a collection; it's not *one* thing, it's an array of *many* things) that your name is pluralized. Also note you *can* store a single object instead of a collection, if your project calls for it.

Why go through all the hassel of all this work, when we could just copy that same object into our working file and save it as a variable? Great question!

## ⚡️⚡️ IT LIVES ⚡️⚡️! (Taking our server live!)

Make sure your terminal is currently in the the root directory of this repo (it should be named json-server-practice unless you named it differently) and type in 
```
json-server --watch db.json
```
This calls the json-server node module installed on your system, and runs the `--watch` command on the file `db.json`. If everything installed correctly and you typed the line correctly you should see your terminal do some thinking and then end on readout that looks surprisingly like this!
```

  \{^_^}/ hi!

  Loading db.json
  Done

  Resources
  http://localhost:3000/users
  http://localhost:3000/books

  Home
  http://localhost:3000

  Type s + enter at any time to create a snapshot of the database
  Watching...
```
By default the `json-server` launches on port 3000, which is accessible in your browser at `http://localhost:3000`. If you visit that location with chrome or your browser of choice now you should get a page that looks like this:
```
Congrats!
You're successfully running JSON Server
✧*｡٩(ˊᗜˋ*)و✧*｡

Resources
/users 2x
/books 3x
To access and modify resources, you can use any HTTP method:

GET POST PUT PATCH DELETE OPTIONS
```
SUCCESS!! We are hosting a server! However, a server sitting by itself doesn't have a lot of practical purposes...let's figure out how we can connect this server to our code! 

Note, you can exit your server at any time by hitting `ctrl+c` in your terminal.

## Interacting with our new DB

If we look at the readout in our terminal, it shows that there are currently 2 "resources" stored in our database, a collection of users and a collection of books, and it tells us how many of each resource are currently saved. 

If we go to `http://localhost:3000/users` or `http://localhost:3000/books` in our browser we can see an array of that specific resource. Go ahead, try it! These would be the routes (often called endpoint) you usually want to target if you are performing a `GET` fetch request, as it will collect all examples of a resource stored at that endpoint and return them as an array.

This is also where you will be performing your `POST` fetches! Whenever we are creating a new User (using our example), we want it to be stored with the rest of the users and have the same "shape" (key:value pairs). This means we *need* to do our `POST` to the endpoint that holds the collection of Users, so the newly created user is held at the same location as the rest of our current resources!

If you want to look at 1 specific, individual resource you can access it by targeting the collection of resources, and then the id of that specific instance. For example, if we wanted to see "Greg's" user object, we can look at his data. He has an "id" of `2`, which means we can go to `http://localhost:3000/users/2`. This lets the database know to look through the users collection, and then actively target the user with an "id" of `2`.

This is necessary for any database interaction that actively targets 1 resource, most notably `PATCH` and `DELETE`. When we tell the database to delete or update a resource, we (usually) only want it to effect a single resource, so we have to let it know which one to target! This means that, more often than not, when you are doing a `PATCH` or `DELETE` that you will be fetching to an endpoint that has a similar structure to `http://localhost:3000/:resources/:id`.

## Success?

That's it! We've built a simple `JSON` database that we can host to mock an externally stored back-end API! If you wanted you could very well stop right here and have a FANTASTIC foundation for a database. Everything I am going to go over from here on out is "sprinkles on the cupcake", nothing you *need* to use `json-server` but things that may make your coding life a little easier in the future.

#### I am going to come back here later maybe and write why we use APIs at all, but want to keep this focused on json-server.


## Port management

By default `json-server` runs on port 3000, but there is a couple of different ways to change that! The simplest is to just add a `--port <desired_port_number>` tag when you run the server! For example, if I wanted my server to run on 3001 instead of 3000, I would start my server with
```
json-server --port 3001 --watch db.json
```
You can also call each of these options with shorthand, the above line runs syntactically identical to the one below
```
json-server -p 3001 -w db.json
```

There is another more permanent solution as well, which is to create a `json-server.json` file on the same level as your `db.json` file, and add the following code inside that file:
```
{
    "port": 3001
}
```
Replace `3001` with whatever port number you'd like, and run your server with:
```
json-server -w db.json
```
You *should* see it now automatically launching on it's newly designated port! Thanks to a recent update by the creator, the `json-server.json` is acting as a mini local configuration file and allows us to add a level of customization to our database.

## Building object relationships

Right now our database has the ability to see all of our `users`, all of our `books`, and any specific `book` or `user` object...but what if we wanted to see our user with the books that *they* own?

If we boot up our server again and look at `http://localhost:3000/books`, we can see that each book has 3 keys.
- `"id"` represents this books stored place in the database, it's an integer, and is unique to that book
- `"title"` represents the title of the book, and it's a string
- `"userId"` This is what we will be focusing on! This is what's called a `foreign key`, and is used to signify that this resource is connected to a different resource. One can think of it as a way for the database to say 
> This book object belongs to a user with the id thats stored in this userId key

We use the naming convention that is required of our library, and for `json-server` the foreign key is camelCase, hence our `user id` being stored as `userId`.

## Nested data and custom routes

Now that we have our data connected, how can we get our server to represent that with our fetches? The answer is custom routes! Let's make a file called `routes.json`, and go ahead and put this code in there:
```
{
    "/users": "/users?_embed=books"
}
```

What is going on here!? Let's tear it apart and figure out:
-  It's a `JSON` file containing an object
- The `key` of that object has a value of `"/users"`, which is one of our endpoints.
- The `value` of that key is `"/users?_embed=books"`, which seems to be calling that same endpoint and also referencing our books resource.

The `key`'s in this object are going to be where we designate the end point that we are customizing. We are telling `json-server` that when it goes to visit `/users`, instead of what it was *GOING* to show us, it will now show us something different.

The `value` for that key represents what the custom route will do instead of it's default action. We are using a built in functionality of `json-server` (that parts of may be syntactically specific to `json-server`) to "include" models with this `GET` request. Here is the core structure for that key value pair:
> "`/endpointParentResource` : "`/endpointParentResource`?_embed=`childResource`"
notice that the `?_embed=` is an unchanging value, and that's what actually connects the two resources **AS LONG AS THE "CHILD" RESOURCE HAS THE PROPER FOREIGN KEY**. The books "belong" to the user, so we **have** to have that `userId` key on the book to connect them.

In this context, we are telling the server 
> When you hit the endpoint `/users`, instead of just showing me an array of users, look through the books and find the ones who's foreign key (`userId`) matches each users id. For all books that match, push them into an array and add them as a new key on this object.

Let's see it in action! To run our `json-server` with routes we need to both call the option as we run the server, and tell it which file is holding those routes. If we've been coding along, now we can type
```
json-server --watch --routes routes.json db.json
```
This is identical to our previous server launch, we just added `--routes routes.json` to tell the server we want to include custom routes that we are storing in the `routes.json` file. If we go to visit our `http://localhost:3000/users` endpoint in the browser, we should see this beautiful array:
```
[
  {
    "id": 1,
    "name": "Francis Felicity Franklin IV",
    "age": 32,
    "books": [
      {
        "id": 1,
        "title": "Coding for Dummies",
        "userId": 1
      },
      {
        "id": 2,
        "title": "<Yoga class=for-programmers/>",
        "userId": 1
      }
    ]
  },
  {
    "id": 2,
    "name": "Greg",
    "age": 38,
    "books": [
      {
        "id": 3,
        "title": "Dirk Gently's Holistic Detective Agency",
        "userId": 2
      }
    ]
  }
]
```

🎉🎊🎉 **SUCCESS** 🎉🎊🎉

This allows us to do a single fetch to get all of our Users, and included within that single fetch is also their associated Books! It's like *magic*.