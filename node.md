# [fit] Node.JS
## [fit] *Why utilizing Node will help you stay ahead of the curve*
![](http://nodejs.org/images/logos/nodejs-2560x1440.png)

---

## *Software platform for scalable server-side and networking applications*

---

# What Is Node
- Software platform built on top of Google's V8 JavaScript engine
- Utilizes event-driven, non-blocking I/O model and single-threaded event loop
- Enables high throughput and application scalability

---

# What Node Is Not
- Node is *NOT* a silver bullet new platform that will dominate the web world
- Instead, it's a platform that fills a particular need

---

# How Node Works
Compared to traditional web-serving techniques where each connection (request) spawns a new thread, taking up system RAM and eventually maxing-out at the amount of RAM available, Node.js operates on a single-thread, using non-blocking I/O calls, allowing it to support support tens of thousands of concurrent connections.

---

![fit](http://www.toptal.com/uploads/blog/image/50/toptal-blog-1_B.png) 

---

# Proof of Concept 
## *Node.js w/1M concurrent connections to a single 15GB rackspace cloud server*
## sysctl dumping the number of open file handles (sockets are file handles):
![inline](http://i1.wp.com/www.caustik.com/blog/1M-handles.PNG)

---

# Advantages
- Easy to spin up a server
- Inexpensive deployment (both cost & time)
- Asynchronocity = High Scalability
- Unified front & backend development

---

# Why Do _We_ Need Node
- Simplifies the process of converting to *service based architecture*
- Enables Real-Time tracking
- Allows us to create data-intensive real-time applications that run across distributed devices

---

- Allows us to employ "push technology" over websockets via real-time two way connections
- No more environment sandboxing & utilizing non-standard ports

---

# [fit] Queued Inputs
## *A relatable Node Use Case*

---

- When receiving high amounts of concurrent data, database can become a bottleneck
- Node.js can easily handle the concurrent connections themselves, but database access is a blocking operation: *BAD*
- __*Solution:*__ Acknowledge client’s behavior before data is written to the database

---

- System maintains its responsiveness under heavy load
- Data gets queued through caching or message queuing infrastructure & digested by separate database batch-write process, or computation intensive processing backend services written in a better performing platform

---

- Similar behavior can be implemented with other languages/frameworks, but not on the same hardware, with the same high, maintained throughput

---

![fit](http://www.toptal.com/uploads/blog/image/53/toptal-blog-3_B.png)

---

# Other Considerations

---

# Permissive Licensing
- https://raw.githubusercontent.com/joyent/node/master/LICENSE
- Node.js packages tend to use extremely permissive licensing, generally MIT or BSD

---

# Data Streaming
- HTTP requests and responses are not independent, isolated events
- Eg: It’s possible to process files while they’re still being uploaded, as the data comes in through a stream and we can process it in an online fashion

---

# Proxying
- Node.js is easily employed as a server-side proxy where it can handle a large amount of simultaneous connections in a non-blocking manner.
- It’s especially useful for proxying different services with different response times, or collecting data from multiple source points

---

# [fit] Performance
## [fit] *Node.js vs Apache + PHP*

---

# The Environment
- Dual-core Intel T4200 2 GHZ machine with 4 GB RAM running Ubuntu 10.04
- ApacheBench 2.3
- Node.js 0.1.103
- Apache 2.2.14 w/ prefork MPM & PHP 5.2.10

---

# The Test
- 100,000 request with 1,000 concurrent requests:

```bash
ab -r -n 100000 -c 1000
```

---

# *JavaScript*
```javascript
var sys = require('sys'), 
http = require('http');
 
http.createServer(function(req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.write('<p>Hello World</p>');
  res.end();
}).listen(8080);
```

---

# *PHP*
```php
<?php
echo '<p>Hello World</p>';
```

---

# Results

---

## *Node.js*

```
Concurrency Level:      1000
Time taken for tests:   21.162 seconds
Complete requests:      100000
Failed requests:        147
   (Connect: 0, Receive: 49, Length: 49, Exceptions: 49)
Write errors:           0
Total transferred:      8096031 bytes
HTML transferred:       1799118 bytes
Requests per second:    4725.43 [#/sec] (mean)
Time per request:       211.621 [ms] (mean)
Time per request:       0.212 [ms] (mean, across all concurrent requests)
Transfer rate:          373.61 [Kbytes/sec] received
```

---

## *Apache*

```
Concurrency Level:      1000
Time taken for tests:   121.451 seconds
Complete requests:      100000
Failed requests:        879
   (Connect: 0, Receive: 156, Length: 567, Exceptions: 156)
Write errors:           0
Total transferred:      29338635 bytes
HTML transferred:       1889607 bytes
Requests per second:    823.38 [#/sec] (mean)
Time per request:       1214.510 [ms] (mean)
Time per request:       1.215 [ms] (mean, across all concurrent requests)
Transfer rate:          235.91 [Kbytes/sec] received
```

---

![100%](http://farm7.static.flickr.com/6005/5891807273_e5f27b9839.jpg)
![100%](http://farm6.static.flickr.com/5310/5892373812_8a67c2a501.jpg)

---

# Concurrency
- Node’s evented architecture is similar to, and influenced by, systems like Ruby’s Event Machine and Python’s Twisted
- Node takes the event model a bit further by presenting the event loop as a language construct instead of as a library

---

- For more heavily trafficked node apps running on multi-core systems, it’s possible to achieve greater concurrency by taking advantage of node core’s cluster module, which automatically load-balances incoming connections across multiple processes
- This allows you to create child processes that share server ports and make better use of dyno resources

---

```javascript
var cluster = require('cluster');
var http = require('http');
var numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  // Fork workers.
  for (var i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', function(worker, code, signal) {
    console.log('worker ' + worker.process.pid + ' died');
  });
} else {
  // Workers can share any TCP connection
  // In this case its a HTTP server
  http.createServer(function(req, res) {
    res.writeHead(200);
    res.end("hello world\n");
  }).listen(8000);
}
```

---

# Asynchronicity
- Everything is Node is asynchronous
- This is quite powerful as it allows a single incoming request to (conceptually) fork off a set of threads at the same time
- In most other environments you program every incoming request serially

---

# Sources
- <https://raw.githubusercontent.com/joyent/node/master/LICENSE>
- <http://opensource.org/licenses/BSD-3-Clause>
- <http://caines.ca/blog/programming/the-node-js-community-is-quietly-changing-the-face-of-open-source/>

---

- <http://info.110consulting.com/blog/bid/352063/The-Benefits-of-Using-Node-js-for-Your-Custom-Application-Development>
- <http://www.toptal.com/nodejs/why-the-hell-would-i-use-node-js>
- <http://blog.caustik.com/2012/08/19/node-js-w1m-concurrent-connections/>

---

- <http://pettergraff.blogspot.com/2013/01/why-node.html>
- <https://devcenter.heroku.com/articles/node-cluster>
- <http://zgadzaj.com/benchmarking-nodejs-basic-performance-tests-against-apache-php>