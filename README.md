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

## API Endpoints

1. `GET /workouts`

- Gets all the workout documents

2. `POST /workouts`

- Creates a new workout document

3.  `GET /workouts/:id`

- Gets a single workout document

4. `DELETE /workouts/:id`

- Deletes a single workout

5. `PATCH /workouts/:id`

- Updates a single workout

### Creating `routes` folder

- this will contain all the request or api for a specific page of your website. preferably all their must be seperate file for each page that handles that request

`workout.js`

```js
const express = require("express");

const router = express.Router();

router.get("/", () => {});

module.exports = router;
//exports the router
```

`server.js`

```js
//require("dotenv").config(); commented out for highlight

//const express = require("express"); commented out for highlight
const workoutRoutes = require("./routes/workout");

//creates an express app
const app = express();

//middleware commented out for highlight
//app.use((req, res, next) => {
//  console.log(req.path, req.method);
//  next;
//});

//Routes
app.use("/workouts", workoutRoutes);

// listen for request
//app.listen(process.env.PORT, () => {
//  console.log(`listening on port 4000!!`);
//});
```

### `express.json()`

- is a middleware function provided by the Express framework in Node.js. It parses incoming requests with JSON payloads and makes the data available in `req.body`.

#### Key Details:

1. **Purpose**:

   - When a client sends data to the server in JSON format (often in the body of a `POST`, `PUT`, or `PATCH` request), `express.json()` processes the JSON payload and converts it into a JavaScript object.

2. **Usage**:

   - You need to include `express.json()` in your Express application to handle JSON request bodies, as Express does not parse JSON by default.

3. **How to Use**:

   - Add `express.json()` as middleware using `app.use()`.

   ```javascript
   const express = require("express");
   const app = express();

   // Enable parsing of JSON request bodies
   app.use(express.json());

   app.post("/data", (req, res) => {
     // Access the parsed JSON data in req.body
     console.log(req.body);
     res.send("JSON data received");
   });

   app.listen(3000, () => {
     console.log("Server running on port 3000");
   });
   ```

4. **When It's Applied**:

   - It processes requests where the `Content-Type` header is `application/json`.
   - If the `Content-Type` is not `application/json`, this middleware does not parse the body.

5. **Examples of a JSON Request**:

   - A client might send a request like this:

     ```http
     POST /data HTTP/1.1
     Host: localhost:3000
     Content-Type: application/json

     {
         "name": "John",
         "age": 30
     }
     ```

   - With `express.json()`, you can access the data in `req.body` as:

     ```javascript
     {
         name: "John",
         age: 30
     }
     ```

6. **Error Handling**:

   - If the JSON is malformed (e.g., missing a closing brace), Express will respond with a `400 Bad Request` error by default, indicating the JSON payload is invalid.

7. **Alternatives**:

   - In older versions of Express (pre-4.16.0), you would use `body-parser`:

     ```javascript
     const bodyParser = require("body-parser");
     app.use(bodyParser.json());
     ```

     Now, `express.json()` is built into Express and doesn't require the `body-parser` package.

### Summary:

`express.json()` is essential for handling requests with JSON payloads. It simplifies the process of extracting and using JSON data in your Express application.

## DATABASE

### Setting up Database

- its better you watch how this is set up [here](https://www.youtube.com/watch?v=s0anSjEeua8&t=132s&ab_channel=NetNinja)
- make sure to do so as these video contains all the bits and piece you need to set up and connect a database

## MODELS and SCHEMA

1. SCHEMA - like a blueprint or plan. It defines the structure of data you want to store in the database, including the fields and their types (e.g., name is a string, age is a number).
2. MODEL - like a tool based on the schema. It lets you interact with the database to create, read, update, and delete the data that follows the schema.

### Mongoose

- after setting up a database and connecting it, you will then install `mongoose` in your backend folder

```shell
npm install mongoose
```

- this will be use to set up the schemas in each model file

### the `models` folder

- this folder will contain all the models of a data that is being send on the data base

here is an example of a model file with schemas:

```js
const mongoose = require("mongoose");

const Schema = mongoose.Schema;

const debtorSchema = new Schema(
  {
    name: {
      type: String,
      required: [true, "Name is required"],
      trim: true,
    },
    contact: {
      type: String,
      trim: true,
    },
    totalBalance: {
      type: Number,
      default: 0,
    },
    debts: {
      type: [Object], // Expecting an array of debt objects
      default: [],
    },
  },
  { timestamps: true }
);

module.exports = mongoose.model("Debtor", debtorSchema);
```

- to understand this better just search for the syntaxes used

you can then used this model to interact with the routes:

```js
const express = require("express");
const Debtor = require("../models/debtorModel.js");

const router = express.Router();

// Test route
router.get("/test", (req, res) => {
  res.send("Debtor route is working!");
});

// GET all debtors
router.get("/", async (req, res) => {
  try {
    const debtors = await Debtor.find();
    res.status(200).json(debtors);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// GET a single debtor by ID
router.get("/:id", async (req, res) => {
  const { id } = req.params;

  try {
    const debtor = await Debtor.findById(id);
    if (!debtor) {
      return res.status(404).json({ message: "Debtor not found" });
    }
    res.status(200).json(debtor);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// CREATE a new debtor
router.post("/", async (req, res) => {
  const { name, contact } = req.body;

  if (!name) {
    return res.status(400).json({ message: "Name is required" });
  }

  try {
    const debtor = await Debtor.create({ name, contact });
    res.status(201).json(debtor);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// UPDATE an existing debtor by ID
router.put("/:id", async (req, res) => {
  const { id } = req.params;
  const { name, contact, totalBalance, debts } = req.body;

  try {
    const updatedDebtor = await Debtor.findByIdAndUpdate(
      id,
      { name, contact, totalBalance, debts },
      { new: true, runValidators: true } // Return updated doc and validate input
    );
    if (!updatedDebtor) {
      return res.status(404).json({ message: "Debtor not found" });
    }
    res.status(200).json(updatedDebtor);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// DELETE a debtor by ID
router.delete("/:id", async (req, res) => {
  const { id } = req.params;

  try {
    const deletedDebtor = await Debtor.findByIdAndDelete(id);
    if (!deletedDebtor) {
      return res.status(404).json({ message: "Debtor not found" });
    }
    res.status(200).json(deletedDebtor);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

module.exports = router;
```

1. **Purpose**: It provides endpoints (URLs) to interact with "debtor" data stored in a database, such as viewing, adding, updating, or deleting debtors.

2. **Key Functions**:

   - **GET `/test`**: Confirms that the debtor routes are working.
   - **GET `/`**: Fetches a list of all debtors.
   - **GET `/:id`**: Fetches details of a specific debtor by their ID.
   - **POST `/`**: Creates a new debtor with data sent in the request body.
   - **PUT `/:id`**: Updates details of a specific debtor by their ID.
   - **DELETE `/:id`**: Deletes a specific debtor by their ID.

3. **Error Handling**: Each route handles potential errors, such as invalid IDs, missing data, or database issues, and returns appropriate status codes and error messages.

4. **Database Model**: It uses the `Debtor` model (imported from `../models/debtorModel.js`) to interact with the database for CRUD (Create, Read, Update, Delete) operations.

5. **Export**: The routes are exported as a module so they can be included in the main application.

## NODE BITS FOR THOSE THAT DOESNT KNOW (ME)

`app.use('/workouts', workoutRoutes);`

- this means that, it'll run the request if you are in this route

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
