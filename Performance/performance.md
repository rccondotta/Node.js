# Performance

## Using the cluster package

Node.js clustering can help improve performance by utilizing all available CPU cores to handle incoming requests. By default, Node.js runs in a single thread, which can become a bottleneck when handling large amounts of traffic.

The cluster module in Node.js allows you to create child processes that share the same server port and handle incoming requests. This can effectively distribute the workload across multiple cores and increase the overall throughput of your application.

Here are some tips for using the cluster module to improve performance:

Identify the parts of your application that can benefit from clustering: Not all parts of your application may be suitable for clustering. Typically, I/O-intensive operations, such as network requests and database queries, are good candidates for clustering.

Create a master process: The master process is responsible for managing the child processes. It creates a pool of worker processes and distributes incoming requests to them. The master process can also monitor the health of the child processes and restart them if they crash.

Create worker processes: The worker processes are the ones that handle incoming requests. They listen for connections on the server port and execute the application logic. Each worker process runs in its own thread, allowing multiple requests to be processed simultaneously.

Use the numCPUs property: The numCPUs property can be used to determine the number of worker processes to create. You can set it to the number of CPU cores available on the machine or adjust it based on the workload.

Use a load balancer: If you have multiple servers running the same Node.js application, you can use a load balancer to distribute incoming requests across them. This can further improve the performance of your application by utilizing multiple machines.

Overall, clustering can be a powerful tool for improving the performance of your Node.js application. By utilizing all available CPU cores and distributing the workload, you can increase the throughput and reduce response times for your users.

## Cluster Example

Here's an example of how to use the `cluster` module in Node.js to create a simple HTTP server that handles incoming requests using multiple worker processes:

```javascript
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  console.log(`Master process ${process.pid} is running`);

  // Fork worker processes
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  // Handle worker process exits
  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker process ${worker.process.pid} died with code ${code} and signal ${signal}`);
    console.log('Starting a new worker process...');
    cluster.fork();
  });
} else {
  // Create HTTP server
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end('Hello World!');
  }).listen(3000);

  console.log(`Worker process ${process.pid} started`);
}
```

In this example, we first check if the current process is the master process using the `cluster.isMaster` property. If it is, we create a pool of worker processes using the `cluster.fork()` method and listen for worker process exits using the `cluster.on('exit', ...)` method. When a worker process dies, we simply create a new one to replace it.

If the current process is not the master process, we create an HTTP server that listens on port 3000 and responds with a "Hello World!" message. We also log the process ID of the worker process.

With this setup, incoming HTTP requests will be distributed across all available worker processes, allowing us to handle more requests simultaneously and improve the performance of our application.

## PM2 Package

Sure! PM2 is a process manager for Node.js that provides several features for improving performance and monitoring your application. Here's an example of how to use PM2 to run a Node.js application:

1. Install PM2 globally using NPM:

    ```
    npm install pm2 -g
    ```

2. Navigate to your Node.js application directory and start the application using PM2:

    ```
    pm2 start app.js
    ```

    This will start the application as a background process and monitor it for crashes and other events.

3. Use PM2 to manage your application:

    - To view the status of your application:

        ```
        pm2 status
        ```

    - To stop your application:

        ```
        pm2 stop app
        ```

    - To restart your application:

        ```
        pm2 restart app
        ```

    - To monitor the logs of your application:

        ```
        pm2 logs
        ```

    - To monitor the CPU and memory usage of your application:

        ```
        pm2 monit
        ```

PM2 also provides several other features, such as load balancing, automatic restarts on crashes, and deployment workflows.

Here's an example of how to use PM2 to run a Node.js application with load balancing:

```javascript
const http = require('http');

http.createServer((req, res) => {
  res.writeHead(200);
  res.end('Hello World!');
}).listen(3000);

console.log(`Worker process ${process.pid} started`);
```

With this simple HTTP server, we can use PM2 to run multiple instances of the application and distribute incoming requests across them.

1. Start the application using PM2:

    ```
    pm2 start app.js -i 4
    ```

This will start 4 instances of the application, which will run on different ports (e.g. 3000, 3001, 3002, 3003).

2. Use a load balancer to distribute incoming requests:

    We can use a load balancer, such as NGINX, to distribute incoming requests across the different instances of the application. Here's an example of an NGINX configuration file that does this:

    ```
    http {
    upstream myapp {
        server localhost:3000;
        server localhost:3001;
        server localhost:3002;
        server localhost:3003;
    }

    server {
        listen 80;

        location / {
        proxy_pass http://myapp;
        }
    }
    }
    ```

With this setup, incoming requests will be distributed across all available instances of the application, allowing us to handle more requests simultaneously and improve the performance of our application.