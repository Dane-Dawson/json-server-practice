# json-server-practice

Mini project to familiarize the use of `json-server`, and practice building a basic `db.json` server and some of it's advanced functionality!

Resources used in writing this tutorial:
I am using https://github.com/typicode/json-server

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
    "name": "Francis Felicity Franklin IV" },
    { "id": 2, 
    "name": "Greg" }
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
By default the `json-server` launches on port 3000,