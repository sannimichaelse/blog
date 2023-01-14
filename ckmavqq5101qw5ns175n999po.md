# REST API Design Best Practices

# Introduction

REST API commonly referred to as Restful API is an Application Programming Interface that defines a set of rules for creating a web service and enables the communication between client and server. Due to the number of growing web services over the internet and the need for an interface for systems to communicate with one another, it becomes pertinent to ensure that the application programming interface is properly engineered for scalability and user-friendliness.

There are lots of Best Practices to follow when architecting your web API. In this article, we will look at a few of them.

## 1. Don't use verbs in URI's

HTTP Verbs like GET, POST, PUT, DELETE already defines the action to be performed on a particular resource. Using verbs in the URI seems to be like a tautology. The resource could be an entity or a particular object that needs to be manipulated and updated. Also, resources should be a Noun. e.g /articles should be used when dealing with creating, fetching, deleting, or updating articles from a database. /articles in this example are the resources which is usually a noun, the action is a GET request on the resources. 

```javascript
const express = require('express');
const bodyParser = require('body-parser');

const app = express();

app.use(bodyParser.json());

app.get('/articles', (req, res) => {
  const articles = [];
  // code to retrieve an article...
  res.json(articles);
});

app.post('/articles', (req, res) => {
  // code to add a new article...
  res.json(req.body);
});

app.put('/articles/:id', (req, res) => {
  const { id } = req.params;
  // code to update an article...
  res.json(req.body);
});

app.delete('/articles/:id', (req, res) => {
  const { id } = req.params;
  // code to delete an article...
  res.json({ deleted: id });
});
```

A typical endpoint for the article's resources would look like the snippet above. 

## 2. API Versioning. 
API's evolve and changes need to made to endpoints on a regular basis. It's always a good practice to version the APIs to keep track of the changes in the endpoint. This allows for backward compatibility and ensures your users are not hitting the wrong version/endpoint. It can be a prefix that can be added to the endpoint. For example, in nestjs, it can be as easy as adding a global prefix to the application

```javascript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  // global prefix
  app.setGlobalPrefix('api/v1');
  await app.listen(3000);
}
bootstrap();
```
## 3. Use plural nouns for resources 
The resources on REST APIs should be named with plural nouns. E.g articles, comments, etc. Most times we usually have more than one entry for the particular resource in our database, so it makes sense to follow convention by depicting the state of the database with the resource definition. It's a standard to ensure that the resources are named using plural nouns to ensure consistency with the state of the database

## 4. Limiting the fields returned by the API. 

It's a good practice for the client application to be able to specify the fields that need to be displayed as opposed to the backend just sending useless and unnecessary data. This speeds up the request and ensures the right data is returned. For example, you can have a fields parameter that takes a list of comma-separated values 

```
GET /articles?fields=id,name,updated_at&state=old&sort=-updated_at
```
The following request would retrieve just enough information to display a sorted listing of old articles:

## 5. Handle errors properly and return standard response codes

Errors in the API should be handled gracefully and the code should depict what went wrong with the request. This allows consumers of the endpoint to know what to look out for and what needs to be fixed. The request could be a bad request from the user or an internal server error from the backend. Irrespective of the cause of the error, the response from the API should be explicit enough to decipher the cause of the problem.

Some standard response codes
```
400 Bad Request – This means that client-side input fails validation.
401 Unauthorized – This means the user is not authorized to access a resource. It usually returns when the user isn’t authenticated.
403 Forbidden – This means the user is authenticated, but not allowed to access a resource.
404 Not Found – This indicates that a resource is not found.
500 Internal server error – This is a generic server error.
502 Bad Gateway – This indicates an invalid response from an upstream server.
503 Service Unavailable – This indicates that something unexpected happened on the server-side (It can be anything like server overload, some parts of the system failed, etc.).
```

## 6. Filtering, Sorting, and Pagination

Databases usually evolve and become large over time, and it makes sense to ensure the API request returns results faster irrespective of the size of the database. Also, it's very important to make sure we don't return all the data in the database as the volume of the data can slow down queries and API requests. It's not a good practice in general to return all the data in the database. For this reason, it's pertinent to add filtering, sorting, and pagination to the request

**Filtering** - This allows the client to be able to request data by a particular entry/field in the database. E.g

```
GET /comments?state=new
```

Here, the state is a query parameter that implements a filter. It's basically saying get the comments that have a new state

**Sorting** - This ensures the data returned from the endpoint is arranged in a particular order, either in descending or ascending format.

```
GET /articles?sort=-updated_at,+name
```

This will return a list of articles sorted by updated_at in descending order and name in ascending other. The **-** sign denotes descending order while the **+** denotes ascending order.

**Pagination** - Sometimes, the client might want to display a very large record on the application. Lets say a list of all the transactions created on an e-commerce platform. Pagination allows the backend to return the data in chunks by specifying a **limit** and **offset** to the request. The limit is the number of entries that would be returned while the offset is where to start from. 

```
GET /articles?offset=10&limit=5
```
This means, get all articles with a limit of 5(only return 5 entries) and an offset of 10(starting from the 10th article). Most times, the result is usually accompanied by a **totalCount** which denotes the count of entries for the request

## 7. API Documentation
Your API is as good as the documentation. Documenting API is of utmost importance as it makes it easy for others to be able to integrate seamlessly with the system. There are several tools you can use for API Documentation
- [Postman Collections](https://www.postman.com/collection/)
- [Swagger.io](https://swagger.io/) etc

# Conclusion

While following standards might not be the silver bullet to all the API problems, you will still need to handle the business logic of the application and ensure the data returned by the request is correct. It's still good to follow standards and best practices as they ensure the architecture of the API follows a consistent format that can be easily understood by other parties and users. There are lots of other better practices, the most important thing is to be consistent with the way the APIs are being built. This will makes it easier for other people to be able to understand the system.




