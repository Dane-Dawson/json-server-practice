# json-server-practice

Mini project to familiarize the use of `json-server`, and practice building a basic `db.json` server and some of it's advanced functionality!

Resources used in writing this tutorial:
I am using https://github.com/typicode/json-server

Quick links

- [What is a JSON Server?](#❓❔-JSON-Server-||-json-server-||-❔❓)
- [Install json-server](#Installing-json-server)
- [Making a database](#It's-database-time!)
- [What's in a database?](#😅-What-did-I-just-do?-😅)
- [Running our server](#⚡️⚡️-IT-LIVES-⚡️⚡️!-(Taking-our-server-live!))
- [json-server interactions](#Interacting-with-our-new-DB)
- [Database complete!](#Success?)

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
#### check naming convention here
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

By default `json-server` runs on port 3000, but there is a couple of different ways to change that! The simplest is to just add a `-p <desired_port_number>` tag when you run the server! For example, if I wanted my server to run on 3001 instead of 3000, I would start my server with
```
json-server --port 3001 --watch db.json
```
You can also call each of these options with shorthand, the above line runs syntactically identical to the one below
```
json-server -p 3001 -w db.json
```

