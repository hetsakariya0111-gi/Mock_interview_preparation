Backend(NodeJS + ExpressJS) Interview Questions to prepare mock interview 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. What is Node.js?
~~~~~~~~~~~~~~~~~~~

A JavaScript runtime built on Chrome's V8 engine, allowing JS to run outside the browser (server-side)
Difference from browser JS: no DOM/window/document; instead has access to file system, OS, networking, modules (fs, http, path)
Browser JS is sandboxed for UI/web pages, Node.js is for building servers, CLI tools, backend logic

2. Asynchronous, non-blocking, single-threaded
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Single-threaded — Node runs JS on a single main thread (the call stack)
Non-blocking — I/O operations (file reads, DB calls, network requests) are delegated elsewhere and don't halt execution
Asynchronous — instead of waiting for an operation to finish, Node continues executing other code and handles results via callbacks/promises when ready
This allows Node to handle many concurrent connections efficiently despite being single-threaded

3. Node.js Architecture: V8 & Libuv
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

V8 Engine — Google's JavaScript engine (also used in Chrome), compiles JS directly to machine code for fast execution
Libuv — a C++ library that provides the event loop, async I/O, and a thread pool for handling operations that can't be non-blocking natively (like file system tasks)
Together: V8 executes JS, Libuv handles async operations and the event loop that makes non-blocking I/O possible

4. Event Loop & its phases
~~~~~~~~~~~~~~~~~~~~~~~~~~

The mechanism that allows Node to perform non-blocking I/O by offloading operations and processing callbacks when ready
Main phases (in order):
Timers — executes setTimeout/setInterval callbacks
Pending callbacks — executes I/O callbacks deferred from previous cycle
Poll — retrieves new I/O events, executes their callbacks
Check — executes setImmediate() callbacks
Close callbacks — handles closed connections (e.g. socket.on('close'))
Microtasks (process.nextTick, Promises) run between each phase, before moving on

5. setImmediate() vs. setTimeout() vs. process.nextTick()
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

process.nextTick() — runs immediately after the current operation, before the event loop continues — highest priority
setTimeout(fn, 0) — runs in the Timers phase, after at least the specified delay
setImmediate() — runs in the Check phase, after the current poll phase completes
Priority order: process.nextTick() > Promises (microtasks) > setTimeout/setImmediate (order between these two can vary depending on context)

6. Thread Pools
~~~~~~~~~~~~~~~

Libuv maintains a thread pool to handle operations that are inherently blocking (like file system operations, DNS lookups, some crypto operations)
Default size: 4 threads
Can be increased via the UV_THREADPOOL_SIZE environment variable

7. Cluster mode vs. Worker threads
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Cluster mode — spawns multiple processes (each with its own memory and event loop) to utilize multiple CPU cores, useful for scaling network/HTTP servers, processes communicate via IPC
Worker threads — run JS in parallel threads within the same process, can share memory (via SharedArrayBuffer), better suited for CPU-intensive tasks
Cluster = multiple processes for scaling servers; Worker threads = true parallel computation within one process

8. Streams in Node.js
~~~~~~~~~~~~~~~~~~~~~

Streams handle reading/writing data piece by piece (chunks) rather than loading it all into memory at once — efficient for large data
Readable — source of data you can read from (e.g. fs.createReadStream)
Writable — destination you can write data to (e.g. fs.createWriteStream)
Duplex — both readable and writable (e.g. a TCP socket)
Transform — a duplex stream that modifies data as it passes through (e.g. compression with zlib)

9. Buffers
~~~~~~~~~~

A Buffer is a temporary storage area for raw binary data, used when working with streams, files, or network protocols
Since JS strings are Unicode-based, Buffers exist to handle raw binary data (like reading a file or image) that isn't naturally text
Fixed-size, allocated outside the V8 heap, and accessed via methods like Buffer.from(), Buffer.alloc()

10. package.json, package-lock.json, ^ vs. ~
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

package.json — defines project metadata, dependencies, scripts, and version info
package-lock.json — locks exact installed versions of every dependency (including nested ones) to ensure consistent installs across machines
^ (caret) — allows updates to minor and patch versions (e.g. ^1.2.3 allows 1.x.x but not 2.0.0)
~ (tilde) — allows only patch version updates (e.g. ~1.2.3 allows 1.2.x but not 1.3.0)

11. CommonJS vs. ES Modules
~~~~~~~~~~~~~~~~~~~~~~~~~~~

CommonJS (require/module.exports) — Node's original module system, synchronous loading, dynamic requires allowed anywhere
ES Modules (import/export) — the official JS standard, asynchronous/static loading, supports tree-shaking, requires .mjs extension or "type": "module" in package.json
ES Modules are becoming the standard, but CommonJS is still widely used, especially in older codebases

12. EventEmitter & custom events
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

EventEmitter is a core Node.js class that enables the publish/subscribe pattern — objects can emit named events that other code listens for
Example:
js
  const EventEmitter = require('events');
  const emitter = new EventEmitter();
  emitter.on('greet', (name) => console.log(`Hello ${name}`));
  emitter.emit('greet', 'John');
Used extensively internally in Node (e.g. streams, HTTP server) and for building custom event-driven architectures

13. Handling uncaughtException & unhandledRejection globally
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

uncaughtException — catches synchronous errors that weren't caught anywhere
js
  process.on('uncaughtException', (err) => { console.error(err); process.exit(1); });
unhandledRejection — catches promise rejections with no .catch() handler
js
  process.on('unhandledRejection', (reason) => { console.error(reason); });
Best practice: log the error and gracefully shut down the process rather than continuing in an unknown state — these are safety nets, not substitutes for proper error handling

14. Express.js & why use it
~~~~~~~~~~~~~~~~~~~~~~~~~~~

A minimal, flexible Node.js web framework for building APIs and web servers
Advantages over native http module: simplified routing, built-in middleware support, cleaner request/response handling, easier error handling, large ecosystem of middleware
Reduces boilerplate significantly compared to raw Node HTTP server code

15. Middleware in Express
~~~~~~~~~~~~~~~~~~~~~~~~~

Functions that execute during the request-response cycle, with access to req, res, and next()
Built-in — e.g. express.json(), express.static()
Third-party — e.g. cors, morgan, helmet
Router-level — middleware applied to specific route groups/routers
Error-handling — special middleware with 4 arguments (err, req, res, next), used to catch and handle errors centrally

16. Central error management via middleware
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Define a single error-handling middleware at the end of the middleware stack:
js
  app.use((err, req, res, next) => {
    res.status(err.status || 500).json({ message: err.message });
  });
Routes/middleware pass errors via next(err) instead of handling them individually
Keeps error formatting and logging consistent across the whole app

17. Routing with Express Router
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

express.Router() lets you create modular, mountable route handlers
js
  const router = express.Router();
  router.get('/users', getUsers);
  app.use('/api', router);
Helps organize routes into separate files/modules, keeping the main app file clean

18. app.use() vs. app.get()/app.post()
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

app.use() — mounts middleware for all HTTP methods, matches a path prefix (or all paths if none specified)
app.get()/app.post() — bind handlers to specific HTTP methods and exact routes
app.use() is more general-purpose (middleware, sub-routers); app.get/post are for specific route logic

19. req.params, req.query, req.body
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

req.params — route/path parameters, e.g. /users/:id → req.params.id
req.query — query string parameters, e.g. /search?term=abc → req.query.term
req.body — data sent in the request body (e.g. POST/PUT), requires body-parsing middleware like express.json()

20. res.send() vs. res.json() vs. res.end() vs. res.download()
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

res.send() — sends a response of various types (string, object, buffer), auto-detects content type
res.json() — explicitly sends a JSON response, sets Content-Type: application/json
res.end() — ends the response process without sending any data (or minimal data), lower-level
res.download() — prompts the client to download a file, sets appropriate headers

21. CORS & resolving CORS errors
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

CORS (Cross-Origin Resource Sharing) — a browser security mechanism that restricts web pages from making requests to a different origin (domain/port/protocol) than the one that served them
Resolved in Express using the cors middleware:
js
  const cors = require('cors');
  app.use(cors({ origin: 'https://example.com' }));
Can configure allowed origins, methods, headers, and credentials as needed

22. Securing an Express app
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Helmet — sets various security-related HTTP headers (e.g. prevents clickjacking, XSS, sniffing) — app.use(helmet())
Rate limiting — restricts number of requests from an IP in a time window to prevent brute-force/DoS attacks (e.g. express-rate-limit)
Data sanitization — cleans user input to prevent injection attacks (e.g. NoSQL injection, XSS) using libraries like express-mongo-sanitize, xss-clean
Other practices: HTTPS, proper CORS config, input validation, environment variable management

23. Environment Variables (.env)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Store configuration values (API keys, DB credentials, secrets) outside the codebase
Loaded via packages like dotenv
Never commit them to GitHub because: they expose sensitive credentials publicly, can lead to security breaches, unauthorized access, or abuse of API keys/services — .env should always be added to .gitignore

24. File uploads with Multer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Multer is Express middleware for handling multipart/form-data, used for file uploads
js
  const multer = require('multer');
  const upload = multer({ dest: 'uploads/' });
  app.post('/upload', upload.single('file'), (req, res) => {
    res.send(req.file);
  });
Supports single/multiple file uploads, storage configuration (disk or memory), and file filtering/validation

25. SQL vs. NoSQL
~~~~~~~~~~~~~~~~~

SQL (Relational) — structured tables with fixed schema, relationships via foreign keys, uses SQL query language, strong consistency (ACID) — e.g. MySQL, PostgreSQL
NoSQL (Non-relational) — flexible/dynamic schema, various models (document, key-value, graph), better horizontal scalability — e.g. MongoDB, Redis
SQL suits structured data with complex relationships; NoSQL suits large-scale, flexible, or rapidly evolving data

26. Mongoose & ODM
~~~~~~~~~~~~~~~~~~

Mongoose is an Object Data Modeling (ODM) library for MongoDB in Node.js
Provides schema definition, validation, middleware (hooks), and a structured way to interact with MongoDB collections
js
  const userSchema = new mongoose.Schema({ name: String, age: Number });
  const User = mongoose.model('User', userSchema);
Adds structure and safety on top of MongoDB's naturally schema-less documents

27. Authentication vs. Authorization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Authentication — verifying who a user is (e.g. login with username/password)
Authorization — determining what an authenticated user is allowed to do/access (e.g. admin vs. regular user permissions)
Authentication happens first, authorization happens after, based on the authenticated identity

28. JWT session management
~~~~~~~~~~~~~~~~~~~~~~~~~~

JWT (JSON Web Token) — a signed token containing user claims, used for stateless authentication
Flow: user logs in → server issues a signed JWT → client sends it in subsequent requests (usually Authorization: Bearer <token>) → server verifies signature to authenticate
Where to store on client:
httpOnly cookies — safer, protected from JS/XSS access, but need CSRF protection
localStorage — simpler, but vulnerable to XSS attacks
httpOnly cookies are generally the recommended, more secure approach

29. Encryption vs. Hashing vs. Encoding
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Encryption — reversible transformation of data using a key, can be decrypted back to original (e.g. AES) — used for protecting data that needs to be read later
Hashing — one-way transformation, cannot be reversed, used for verifying data integrity or storing passwords (e.g. bcrypt) — same input always produces same hash
Encoding — transforms data into a different format for compatibility/transmission (e.g. Base64), not for security, easily reversible without any key

30. REST API best practices & HTTP status codes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Best practices: use nouns for resource URLs (/users not /getUsers), proper HTTP methods (GET/POST/PUT/DELETE), consistent response structure, versioning (/api/v1/), proper status codes, pagination for large datasets, authentication/authorization, meaningful error messages
Status codes:
200 OK — successful request
201 Created — resource successfully created
400 Bad Request — invalid client request/input
401 Unauthorized — authentication required/failed
403 Forbidden — authenticated but not permitted to access
404 Not Found — resource doesn't exist
500 Internal Server Error — unexpected server-side failure

