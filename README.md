## SETTING UP BACKEND

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

## NODE BITS FOR THOSE THAT DOESNT KNOW (ME)

#### `app.get(path, callback)`

`app:` The Express application instance.
`path: `The URL path (e.g., / or /users) to listen for GET requests.
`callback:` A function executed when the specified path is requested. It has two arguments:
`req (request):` Contains information about the incoming request.
`res (response):` Used to send a response to the client.
