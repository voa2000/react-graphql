# React-GraphQL

## GraphQL is a query language for your API, and a server-side runtime for executing queries by using a type system you define for your data. GraphQL isn't tied to any specific database or storage engine and is instead backed by your existing code and data.
Learn more about GraphQL here https://graphql.org/learn/

GrapQL implementation with express documentation https://github.com/graphql/express-graphql

Documentation of API used in this project https://github.com/r-spacex/SpaceX-API

Promise based HTTP client for the browser and node.js https://github.com/axios/axios

Why GitHub is using GraphQL . https://developer.github.com/v4/

REST vs. GraphQL: A Critical Review   https://goodapi.co/blog/rest-vs-graphql

GraphQL Server Playground https://launchpad.graphql.com/new
GitHub page for Lauchpad playground https://github.com/apollographql/awesome-launchpad 

The payload is the part of that response that is communicating directly to you. In REST APIs this is usually some JSON formatted data. ... You get back an JSON object with a link to a cat picture along with a few other pieces of information. That JSON is the payload.
# Development of GraphQL on an express server in Node.
Intialise Node backend server
```npm init```
Install dependencies
`npm i graphql express-graphql express axios`
Edit JSON file to start backend server
`"scripts": {
    "start": "node Server.js"`
Create backend server in Server.js using express implementation from documentation about GraphQL on GitHub
```
const express = require('express');
const graphqlHTTP = require('express-graphql');

const app = express();

app.use('/graphql', graphqlHTTP({
  schema: MyGraphQLSchema,
  graphiql: true
}));

app.listen(4000);
```
