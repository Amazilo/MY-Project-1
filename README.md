# MY-Project-1
Multi-Stack DevOps Infrastructure Automation
This project will help enable your knowledge in the process of working with different programming languages, two types of databases, and overall communications and security in a private network.

Multiple Micro-Services Voting Application
This application is intentionally a “polyglot” stack. It’s not an example of a perfect architecture, but rather a way to expose you to:

Multiple languages (Python, Node.js, C#/.NET)
Multiple frameworks (Flask, Express, .NET Worker)
Multiple data stores (Redis, Postgres)
Dockerization of diverse applications
The original project is taken from Docker Samples—we created a few tweaks for simplicity.

App Overview
The app is a micro-services app where users can cast their favourite pet : cats or dogs. We have a vote page and a result page. These are the two services that are meant to be seen by the end-user. The other micro-services are meant to be private and handled only internally, to enable security.

Micro-Services Overview
Vote Application: A Python/Flask-based frontend where users cast their votes. It runs on Python, exposes an HTTP interface, and stores or retrieves votes via Redis. The app.py file contains the Flask application.

Redis: An in-memory database used as a queue for processing votes. If you receive 2000 votes in 5 seconds, redis will process them way faster than a SQL database.

Result: A Node.js application that displays results in real-time. The server.js file is the main entrypoint, serving a web page with Angular.js components that connect to a WebSocket.

Worker: A .NET (C#) application that processes the votes and stores them in a database. This application consumes votes from Redis and writes them into PostgreSQL. The Program.cs and Worker.cs files define a background worker process.

PostgreSQL: Stores persistent vote data that rets retrieved by Result.

Architecture Diagram

Connections Overview
The worker expects a running Redis and PostgreSQL instance to function correctly. Without them, it will likely fail to connect or idle while waiting.

Note: There are a total of four connections for the whole network of applications to run correctly:

Vote → redis
Worker → redis
Worker → postgresql
Result → postgresql
This also means that:

The container vote needs 1 environment variable to point to redis;
The container result needs 1 environment variable to point to postgres;
The container worker needs 2 environment variables – one for redis and another one for postgresql;
Having local/remote targets is a great start to really grasp different kinds of connections, within Docker Network and within a Private Network. Since we have different instances, chances are we might have to call a Public IP instead of a container name. If we map ports from the container to the host, then the container becomes available to the outside world, too.
