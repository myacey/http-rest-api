# HTTP REST API

This project is a small HTTP REST API server written in Go.

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Configuration](#configuration)
- [Database Setup](#database-setup)
- [Running the Server](#running-the-server)
- [Usage](#usage)
- [Running Tests](#running-tests)

## Introduction

This project is a small HTTP REST API server written in Go. It uses the `net/http` package to handle HTTP requests and responses, `gorilla/mux` for the router. Passwords are securely hashed using the `bcrypt` algorithm to ensure user security. The server also manages sessions by storing cookies (`gorilla/sessions`).

## Installation

To get started, clone the repository and navigate to the project directory:


``` sh
git clone https://github.com/myacey/http-rest-api.git 
cd http-rest-api
```
Ensure you have Go installed on your machine. You can download it from [golang.org](https://golang.org/dl/).

## Configuration
Before running the server, you need to configure your database connection. Update the database_url in `toml` in the configuration file with your database credentials:
``` toml
database_url = "host=localhost dbname=restapi_dev sslmode=disable user=<your_name> password=<your_password>"
```

## Database Setup

The project uses a PostgreSQL database. You need to create and migrate the database manually.

1. Create the database:
	``` sh
	createdb resapi_dev
	```
    
2. Run migrations:
	``` sh
	migrate create -ext sql -dir migrations 
	create_user migrate -path migrations -database "postgres://localhost/resapi_dev?sslmode=disable" up 
	```
## Running the Server

Build and run the server using `make`:
```
make
./apiserver
```

## Usage
You can interact with the API using tools like HTTPie. Ensure you have HTTPie installed on your machine.

To install HTTPie, you can follow the instructions on their official website.

### Example Commands
To create a new user, use the following command:
``` sh
http --ignore-stdin POST localhost:8080/users email=user@example.org password=password
```

To create a new session, use the following command:
``` sh
http -v --ignore-stdin POST localhost:8080/sessions email=user@example.org password=password
```

Also you can check cookies with:
``` sh
http -v --session=user localhost:8080/private/whoami "Origin: google.com"
```
Answer example:
``` sh
GET /private/whoami HTTP/1.1
Accept: application/json, */*;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Cookie: JXGERcorp=MTcyMTM5MTM5OHxEWDhFQVFMX2dBQUJFQUVRQUFBZV80QUFBUVp6ZEhKcGJtY01DUUFIZFhObGNsOXBaQU5wYm5RRUFnQUd8mjrhdl_KG6grAccQiEdJMnaovyqrgrK5-AJIJLuTBSo=
Host: localhost:8080
Origin: google.com
User-Agent: HTTPie/3.2.2



HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Content-Length: 36
Content-Type: text/plain; charset=utf-8
Date: Mon, 22 Jul 2024 12:46:20 GMT
X-Request-Id: 2b08cb4f-4280-4357-acca-87953ddacdfd

{
"email": "user@example.org",
"id": 3
}
```

## Running Tests

To run tests, use the following command:
``` sh
make test
```
