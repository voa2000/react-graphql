# React-GraphQL

## GraphQL is a query language for your API, and a server-side runtime for executing queries by using a type system you define for your data. GraphQL isn't tied to any specific database or storage engine and is instead backed by your existing code and data.

-  Query
.  Every GraphQL schema has a root type for both queries and mutations. The query type defines GraphQL operations that retrieve data from the server.
-  Mutation
.  The mutation type defines GraphQL operations that change data on the server. It is analogous to performing HTTP verbs such as POST, PATCH, and DELETE.

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
`npm i graphql express-graphql express axios nodemon`
3.  Edit JSON file to start backend server
```
"scripts": {
    "start": "node Server.js",
    "server": "nodemon Server.js"
  }
  ```
4.  Create backend server in Server.js using express implementation from documentation about GraphQL on GitHub
```
const express = require("express");
const graphqlHTTP = require("express-graphql");
const schema = require("./schema");
const app = express();

app.use(
  "/graphql",
  graphqlHTTP({
    schema,
    graphiql: true
  })
);
const PORT = process.env.PORT || 5000;
app.listen(PORT, () =>
  console.log(`Server started on port ${PORT}
      Go to localhost:5000/graphql to access the endpoint`)
);
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
8.  The implementation above uses Apollo which supports both back and frontend implementations.
Facebook who created this specification use Relay.

#  Summary
-  GraphQL is a specification and not an implementation.
-  A specification describes how a product should look and work, this structure makes it easy to work with database and APIs.
-  It favours fetching the data required therefore stops over fetching good for devices with less memory.
-  All data can be fetched from one endpoint for all devices and uses as the queries will return only the data you want.

#  Next steps
-  Create React App frontend client to work concurrently with this backend.
-  Using GraphQL with Arrays in NodeJS to include Mutations.
-  Using GraphQL with MongoDB with Queries and Mutations.
