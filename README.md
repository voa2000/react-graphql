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
1.  Intialise Node backend server
```npm init```
2.  Install dependencies
`npm i graphql express-graphql express axios`
3.  Edit JSON file to start backend server
`"scripts": {
    "start": "node Server.js"`
4.  Create backend server in Server.js using express implementation from documentation about GraphQL on GitHub
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
5.  Start backend server
`npm start`
6.  Create schema to load data and create queries. Save file as schema.js
```
const axios = require("axios");
const {
  GraphQLObjectType,
  GraphQLBoolean,
  GraphQLInt,
  GraphQLString,
  GraphQLSchema,
  GraphQLList
} = require("graphql");
// Launch Type
const LaunchType = new GraphQLObjectType({
  name: "Launch",
  fields: () => ({
    flight_number: { type: GraphQLInt },
    mission_name: { type: GraphQLString },
    launch_year: { type: GraphQLString },
    launch_date_local: { type: GraphQLString },
    launch_success: { type: GraphQLBoolean },
    rocket: { type: RocketType }
  })
});
// Rocket Type
const RocketType = new GraphQLObjectType({
  name: "Rocket",
  fields: () => ({
    rocket_id: { type: GraphQLString },
    rocket_name: { type: GraphQLString },
    rocket_type: { type: GraphQLString }
  })
});
// Root Query with resolvers
const RootQuery = new GraphQLObjectType({
  name: "RootQueryType",
  fields: {
    launches: {
      type: new GraphQLList(LaunchType),
      resolve(parent, args) {
        return axios
          .get("https://api.spacexdata.com/v3/launches")
          .then(response => response.data);
      }
    },
    launch: {
      type: LaunchType,
      args: {
        flight_number: { type: GraphQLInt }
      },
      resolve(parent, args) {
        return axios
          .get(`https://api.spacexdata.com/v3/launches/${args.flight_number}`)
          .then(response => response.data);
      }
    }
  }
});
module.exports = new GraphQLSchema({
  query: RootQuery
});
```
7.  Visit GraphQL endpoint to visualise data and query data.  This endpoint has been set in your Server.js file.
```http://localhost:5000/graphql ```
