# MERN COURSE

## EXPRESS APP SET UP

### Starting

1. make a folder named `backend`
2. create a file `server.js` this will be the entry point for the server
3. open terminal and execute the following command

```shell
cd backend

npm init -y

npm intall express
```

### Setting up `server.js`

```js
const express = require("express");

//creates an express app
const app = express();

// listen for request
app.listen(4000, () => {
  console.log(`listening on port 4000!!!`);
});
```

to run this server you need to download nodemon globally, this is so we can use nodemon in the computer. ONLY DO THIS IF YOU HAVENT YET:

```shell
npm install -g nodemon
```

NODEMON ALLOWS FOR INSTANT CHANGES WHEN A FILE CHANGES AND RERUNS FOR YOU WHICH IS COOL

### Setting up `.env`

- `.env` allows us to hide sensitive information about from the public or users of the site. typically where the port is. so when pushing this to a github you must add it to `git ignore`

- install `dotenv` first:

```shell
cd backend

npm install dotenv
```

- we will then require the .env by doing the following:

```js
require("dotenv").config();

const express = require("express");

//creates an express app
const app = express();

//Routes
app.get("/", (request, response) => {
  response.json({ mssg: "Welcome to the app" });
});

// listen for request
app.listen(process.env.PORT, () => {
  console.log(`listening on port 4000!!`);
});
```

### POSTMAN

- simulates different types of request on a server
- download it [here](https://www.postman.com/downloads/)

### MIDDLEWARE

- codes that executes between us getting a request on the server and of sending a response

The statement `app.use((req, res, next) => {})` in Node.js is used to define middleware in an Express application. Here's what it does in detail:

### Components:

1. **`app.use()`**:

   - This method is used to register middleware functions in an Express application.
   - Middleware functions are executed sequentially for every incoming request to the server.
   - Middleware can perform tasks like logging, authentication, data processing, and error handling.

2. **Callback Function Parameters**:
   - **`req`**: The request object, representing the incoming HTTP request. It contains details like the URL, headers, query parameters, body, etc.
   - **`res`**: The response object, which is used to send a response back to the client.
   - **`next`**: A function that passes control to the next middleware in the chain. If `next()` is not called, the request will not proceed further.

### Purpose:

The middleware function inside `app.use()` runs on **every incoming request** unless a specific path is provided (e.g., `app.use('/path', ...)`).

### Example:

```javascript
const express = require("express");
const app = express();

// Middleware function to log request details
app.use((req, res, next) => {
  console.log(`Request Method: ${req.method}, Request URL: ${req.url}`);
  next(); // Pass control to the next middleware or route
});

// Example route
app.get("/", (req, res) => {
  res.send("Hello, World!");
});

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```

### Explanation of the Example:

1. The middleware function logs the HTTP method (`req.method`) and URL (`req.url`) of every incoming request.
2. The `next()` function is called to ensure the request continues to the next middleware or route handler.
3. If `next()` wasn't called, the request would hang, as no response would be sent.

### Key Notes:

- Middleware is executed in the order it is defined.
- Middleware can **modify the request and response objects**.
- Some middleware doesn't need to call `next()` if it sends a response (e.g., error-handling middleware).

This pattern is the backbone of how Express handles requests and responses in a modular way.

## NODE BITS FOR THOSE THAT DOESNT KNOW (ME)

#### `app.get(path, callback)`

1. `app:` The Express application instance.
2. `path: `The URL path (e.g., / or /users) to listen for GET requests.
3. `callback:` A function executed when the specified path is requested. It has two arguments:
4. `req (request):` Contains information about the incoming request.
5. `res (response):` Used to send a response to the client.

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello, World!"); // Sends a response to the client
});

app.get("/users", (req, res) => {
  res.json({ name: "John Doe", age: 30 }); // Sends JSON data
});

app.listen(3000, () => {
  console.log("Server is running on http://localhost:3000");
});
```
