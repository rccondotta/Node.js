# Web Server

## What is a Web Sever

In Node.js, a web server is a software application that listens for incoming HTTP requests from clients (such as web browsers), and responds to them by serving content (such as HTML pages, images, or other resources).

Node.js provides built-in modules like http and https that allow developers to easily create and run web servers. These modules provide APIs for creating and configuring servers, handling incoming requests, and sending responses back to clients.

For example, a simple Node.js web server could be created using the following code:

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

This code creates an HTTP server that listens for incoming requests on port 3000. When a request is received, the server sends back a plain text response of "Hello, World!".

## Http Requests and Rsponses

HTTP requests and responses are the standard way for clients (such as web browsers) and servers to communicate over the web. In Node.js, the http and https modules provide APIs for handling HTTP requests and responses.

HTTP requests are made by clients and contain information about the resource that the client wants to access. A typical HTTP request includes a method (such as GET, POST, or PUT), a URL, headers, and optionally a message body. Here's an example of making an HTTP GET request in Node.js using the built-in http module:

```javascript
const http = require('http');

const options = {
  hostname: 'example.com',
  port: 80,
  path: '/',
  method: 'GET'
};

const req = http.request(options, res => {
  console.log(`statusCode: ${res.statusCode}`);
  
  res.on('data', d => {
    process.stdout.write(d);
  });
});

req.on('error', error => {
  console.error(error);
});

req.end();
```
This code sends an HTTP GET request to example.com and logs the response status code and body to the console.

HTTP responses are sent by servers and contain the requested resource along with additional metadata such as status codes, headers, and cookies. In Node.js, an HTTP response can be created using the http module's ServerResponse class. Here's an example of sending an HTTP response in Node.js:

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

This code creates an HTTP server that sends an HTTP response with a status code of 200 and a plain text body of "Hello, World!" for every incoming request.

## Http APIs and Routing

HTTP APIs and routing are commonly used in Node.js to create web applications that expose an API for other services to consume. In Node.js, the Express framework is a popular choice for building HTTP APIs and routing.

HTTP APIs typically use HTTP methods (such as GET, POST, PUT, and DELETE) to expose a set of endpoints that can be accessed by clients. Each endpoint typically represents a resource or an operation that can be performed on a resource. For example, an HTTP API for a blog might expose endpoints for creating, reading, updating, and deleting blog posts.

In Express, HTTP routing is used to map incoming requests to specific endpoint functions. Endpoint functions are typically defined as a combination of an HTTP method and a URL path. For example, the following code defines an endpoint function for handling HTTP GET requests to the /users path:

```javascript
const express = require('express');
const app = express();

app.get('/users', (req, res) => {
  // Handle GET /users requests here
});
```
In this example, the app.get() function is used to define an endpoint function for handling HTTP GET requests to the /users path. When a client makes a GET request to the /users path, the function passed as the second argument to app.get() will be called.

Endpoint functions typically use the req and res parameters to access information about the incoming request and to send a response back to the client. For example, the following code sends a JSON response back to the client with a list of users:

```javascript
const express = require('express');
const app = express();

const users = [
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' }
];

app.get('/users', (req, res) => {
  res.json(users);
});
```

In this example, the res.json() function is used to send a JSON response back to the client with the users array as its payload. The json() function sets the appropriate Content-Type header and converts the response data to JSON.

## Paramaterized URLs

Parameterized URLs, also known as URL parameters, are a way to add variable parts to URL paths in an HTTP API. Parameterized URLs are commonly used in RESTful APIs to represent resources that can be identified by a unique identifier or a set of parameters.

In Node.js with the Express framework, parameterized URLs can be defined using route parameters. Route parameters are defined by prefixing a URL path segment with a colon (:) followed by a parameter name. For example, the following code defines an endpoint function for handling HTTP GET requests to a parameterized URL path:

```javascript
const express = require('express');
const app = express();

app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  // Handle GET /users/:id requests here
});
```
In this example, the app.get() function is used to define an endpoint function for handling HTTP GET requests to a parameterized URL path /users/:id. The :id part of the URL path is a route parameter that can match any string in the URL path. When a client makes a GET request to a URL like /users/123, the :id parameter will be set to 123.

Route parameters can be accessed in the endpoint function using the req.params object. The req.params object is a key-value map that maps parameter names to their values in the URL path. In the example above, the userId variable is set to the value of the :id parameter.

```javascript
const express = require('express');
const app = express();

app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  res.send(`User ID: ${userId}`);
});
```

In this example, the endpoint function sends a plain text response back to the client with the value of the :id parameter in the URL path. If a client makes a GET request to /users/123, the response body will be User ID: 123.

## Same Origin Policy and Cross Origin

The Same Origin Policy is a security feature implemented by web browsers to restrict web pages from making requests to a different domain than the one that served the page. The policy is designed to prevent malicious websites from stealing sensitive information from other websites that a user might be logged into.

Under the Same Origin Policy, a web page can only make requests to the same domain, port, and protocol as the page itself. For example, a web page loaded from https://www.example.com can only make requests to resources on https://www.example.com, not to https://www.otherdomain.com.

In Node.js, the Same Origin Policy is not enforced by default, as Node.js is primarily used for server-side development and does not typically serve web pages. However, it is still important to consider the Same Origin Policy when building HTTP APIs or other web services that might be consumed by web pages.

To enable cross-origin requests in a Node.js application, the server can set the appropriate CORS headers in its responses. CORS (Cross-Origin Resource Sharing) is a mechanism that allows a server to specify which domains are allowed to make cross-origin requests. The CORS headers include Access-Control-Allow-Origin, which specifies the domains that are allowed to access the server's resources.

For example, the following code uses the cors middleware package to enable cross-origin requests in an Express application:

```javascript
const express = require('express');
const cors = require('cors');

const app = express();

app.use(cors());

app.get('/users', (req, res) => {
  // Handle GET /users requests here
});
```
In this example, the cors() middleware function is used to enable cross-origin requests for all routes in the application. This will add the appropriate CORS headers to the server's responses to allow cross-origin requests from any domain.

It is important to use caution when enabling cross-origin requests, as it can introduce security vulnerabilities if not properly configured. The cors middleware package provides various options for fine-tuning the CORS policy to fit the specific needs of an application.

## Requests and Responses as Streams

In Node.js, HTTP requests and responses can be treated as streams, which means that they can be read and written in a chunked, streaming manner rather than as a single block of data.

The http module in Node.js provides the http.IncomingMessage class for representing incoming HTTP requests, and the http.ServerResponse class for representing outgoing HTTP responses. Both of these classes are streams that can be read and written to in a streaming fashion.

For example, to read the request body as a stream, you can listen to the data and end events on the IncomingMessage object. The data event is emitted each time a chunk of data is received, and the end event is emitted when the entire request body has been received.

```javascript
const http = require('http');

http.createServer((req, res) => {
  let requestBody = '';

  req.on('data', (chunk) => {
    requestBody += chunk;
  });

  req.on('end', () => {
    console.log(`Received request body: ${requestBody}`);
    res.end('Hello, World!');
  });
}).listen(3000);
```
In this example, the req.on('data', ...) and req.on('end', ...) methods are used to listen for incoming request data. Each time a chunk of data is received, it is added to the requestBody variable. When the entire request body has been received, the end event is emitted and the server sends a response.

Similarly, to write a response body as a stream, you can use the write() method on the ServerResponse object to write chunks of data, and the end() method to signal the end of the response.

```javascript
const http = require('http');

http.createServer((req, res) => {
  res.write('Hello, ');
  setTimeout(() => {
    res.write('World!');
    res.end();
  }, 1000);
}).listen(3000);
```

In this example, the server sends the response in two chunks. First, it sends the string 'Hello, ', and then it waits for 1 second using setTimeout() before sending the string 'World!' and signaling the end of the response using res.end().