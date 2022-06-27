---
layout: default
title:  'What is express.js?'
parent: Javascript
nav_order: 5
description: "A breif explaination of express.js, a framework for NodeJS and how to use it."
---

## A fast and minimal framework for Node.js

[Express.js](https://expressjs.com) is one of the most popular Node frameworks.

### How to install express from the command line

Start from creating a directory.

```js
mkdir directoryname // create a directory with a desired name.
cd directoryname // change directory to the created one.
npm init // create a package.json file.
```

```js
entry point: (app.js) // enter the file name you want node to read as entry point.
```

```js
npm install express --save // install express in the directory and save it as a dependency.
npm install express --no-save // install express temporarily without save it as a dependency.
```

### A simple express example

```js
const express = require('express' 4.16.4); // save express module with version in a constant named express.
const app = express(); // save express function in app as convention.
const port = 5000; // specify a port number.

app.get('/', (req, res) => res.send('This is Express')); // responds with "This is Express" to the browser."

app.listen(port, () => console.log(`Example app is listening on port ${port}!`));
```

This will start a server and listens on port 5000 for connections responding with ```This is Express``` for requests to the root URL ```'/'``` or ```route```. It will respond with a 404 Not Found for other paths.

### To run the app locally

```js
node app.js
```

## What is Express application generator

It is for rapidly creating an app skeleton. For installing, use:

```js
npm install express-generator -g
```

This app generator comes with template or view engine called ```Pug```. There are also other view engine supports such as ```ejs```, ```handlebars```, and ```hogan.js```. However, it is possible to generate without a view engine by using ```--no-view```.

The following command will create an Express app named ```expressapp```. The app will be created in the ```expressapp``` folder in the current working directory and view engine will be ```Pug```.

```js
express --view=pug expressapp
```

To install dependencies:

```js
cd expressapp
npm install
```

To run and app on MacOS or Linux:

```js
DEBUG=expressapp:* npm start
```

To run and app on Windows:

```js
set DEBUG=expressapp:* & npm start
```

Open up <http://localhost:3000/> in your browser to access the app.

Here is the directory structure:

```js
.
├─ app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.pug
    ├── index.pug
    └── layout.pug

7 directories, 9 files
```

## Basic Routing

```js
app.Method(PATH, HANDLER)
```

Where:

* app is an instance of express.
* METHOD is an HTTP request method, in lowercase.
* PATH is a path on the server.
* HANDLER is the function executed when the route is matched.

## Serving static files in Express

Files such as images, CSS, and JS can be served uing the ```express.static``` build-in middleware function in Express.

```js
express.static(root, [options])
```

The root argument specifies the root directory from which to serve static assets. Click [express.static](https://expressjs.com/en/4x/api.html#express.static) for more information on the options argument.

The following code serves images, CSS, and JS files in a directory named ```public```:

```js
app.use(express.static('public'))
```

Assuming these files are in public directory:

```js
http://localhost:3000/images/mountain.jpg
http://localhost:3000/css/custom.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg-white.png
http://localhost:3000/about.html
```

To use multiple static assets directories, call the express.static middleware function multiple times:

```js
app.use(express.static('public'));
app.use(express.static('files'));
```

> NOTE: For best results, use a reverser proxy cache to improve performance of serving static assets.

To create a virtual path prefix (where the path does not actually exist in the file system) for files that are served by the `express.static` function, specify a mount path for the static directory, as shown below:

```js
app.use('/static', express.static('public'));
```

Now, you can load the files that are in the public directory from the `static` path prefix.

```js
http://localhost:3000/static/images/mountain.jpg
http://localhost:3000/static/css/custom.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg-white.png
http://localhost:3000/static/about.html
```

However, the path that you provide to the `express.static` function is relative to the directory from where you launch your `node` process. If you run the express app from another directory, it's safer to use the absolute path of the directory that you want to serve:

```js
app.use('/static', express.static(path.join(__dirname, 'public')));
```

The following example illustrate defining REST APIs with the use of async and await:

```js
const express = require('express');
const app = express();

const records = require('./records');

function asyncHandler(cb) {
  return async (req, res, next) => {
    try {
      await cb(req, res, next);
    } catch (err) {
      next(err);
    }
  }
}

app.use(express.json());

// Send a GET request to /quotes to READ a list of quotes
app.get('/quotes', async (req, res) => {
  try {
    const quotes = await records.getQuotes();
    res.json(quotes);
  } catch (err) {
    res.status(500).json({
      message: err.message
    });
  }
});
// Send a GET request to /quotes/:id to READ(view) a quote
app.get('/quotes/:id', async (req, res) => {
  try {
    const quote = await records.getQuote(req.params.id);
    if (quote) {
      res.json(quote);
    } else {
      res.status(404).json({
        message: "Quote not found."
      });
    }
  } catch (err) {
    res.status(500).json({
      message: err.message
    });
  }
});

// Send a POST request to /quotes to CREATE a new quote
app.post('/quotes', async (req, res) => {
  try {
    if (req.body.author && req.body.quote) {
      const quote = await records.createQuote({
        quote: req.body.quote,
        author: req.body.author
      });
      res.status(201).json(quote);
    } else {
      res.status(400).json({
        message: "Quote and author required."
      });
    }
  } catch (err) {
    res.status(500).json({
      message: err.message
    });
  }
});
// Send a PUT request to /quotes/:id to UPDATE (edit) a quote
app.put('/quotes/:id', async (req, res) => {
  try {
    const quote = await records.getQuote(req.params.id);
    if (quote) {
      quote.quote = req.body.quote;
      quote.author = req.body.author;

      await records.updateQuote(quote);
      res.status(204).end();
    } else {
      res.status(404).json({
        message: "Quote Not FOUND"
      });
    }

  } catch (err) {
    res.status(500).json({
      message: err.message
    });
  }
})
// Send a DELETE request to /quotes/:id DELETE a quote
app.delete('/quotes/:id', async (req, res) => {
  try {
    const quote = await records.getQuote(req.params.id);
    await records.deleteQuote(quote);
    res.status(204).end();
  } catch (err) {
    next(err);
  }
});
// Send a GET request to /quotes/quote/random to READ (view) a random quote

app.use((req, res, next) => {
  const err = new Error("Not Found");
  err.status = 404;
  next(err);
});

app.use((err, req, res, next) => {
  res.status(err.status || 500);
  res.json({
    error: {
      message: err.message
    }
  })
});

app.listen(3300, () => console.log('Quote API listening on port 3300!'));
```