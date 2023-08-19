# Tutorial
### How to enable CORS on an Express JS application, the quick way.
When making an Express JS app, i found a heavy roadblock whenever i tried to learn how to connect my frontend to my expressjs app, after testing succesfully from Postman, losing a lot of time.

So, here is a quick reference to all new backend developers learners out there. Is not the best way but is a quick one to overcome the frustration of CORS policy errors.

## Introduction
CORS, or Cross-Origin Resource Sharing, is a mechanism that allows many resources (e.g., fonts, JavaScript, etc.) on a web page to be requested from another domain outside the domain from which the resource originated. This can be useful in situations where you have separated your frontend and backend servers, and need to talk between the two over a domain boundary.

In an HTTP request, the browser sends origin (the source or the domain where the request started) information in headers. The server checks these headers and if the server accepts the request from this origin, it responds with the Access-Control-Allow-Origin header to tell the browser that the cross-origin traffic is approved. 

But for some types of requests, called "not simple", browsers send two requests, not one. The first is a special "preflight" OPTIONS request, to check if the server would allow the main request.

How does this work in an express.js app? 

1. You install the npm package called `cors`.

2. After that you can import it into your application using `require('cors')`.

3. Then you can use `app.use(cors())` to apply the CORS middleware to every request. This will automatically add the appropriate headers that "tell" the browser if and how cross

## Installation
Just run:
```bash
npm install cors --save
```

## Configuration
Put on your application your import and the app.use method to use this middleware, after your express import:
```javascript
import express from 'express';
import cors from 'cors';

// You can declare the CORS middleware options here 
// if you want to allow just some origins for your 
// express app to serve and block all the other 
// origins, is the recommended way. But you can omit 
// this if you are working on dev.

const corsOptions={
    origin:[
        'http://localhost:3000',
        'https://yourwebsite'
    ],
    optionsSucessStatus: 200
}
// Remember to add your local development server port

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(cors(corsOptions)); 
// You can omit corsOptions and put nothing (Not recommended)
```

And that's all. 

## Considerations
CORS use the OPTIONS method to make a **Preflight Request**, so avoid using this method to any other thing on your app. 

When a web application tries to make a cross-origin request that is considered 'not simple' (such as DELETE requests, or POST requests with a content-type other than application/x-www-form-urlencoded, multipart/form-data, or text/plain), web browsers automatically first send an HTTP OPTIONS request to the server, to check if the server supports the actual request the client wants to send. This OPTIONS request is what's referred to as a "preflight request".

In order for the actual request to be allowed, the server must respond to this OPTIONS preflight request with appropriate CORS headers that indicate that the actual request is allowed.

In express.js applications you typically don't have to manually handle OPTIONS requests if you are using the `cors` middleware, as this package correctly handles OPTIONS preflight requests and sets the appropriate headers for you.
