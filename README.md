## Cadidate: Danh Nguyen Hoang - nguyendanhtravel@gmail.com
### I. My improvement
#### 1. Split `services` layer to `use case` and `transport` layer
#### 2. Change to use `chi-go router`. Why chi-go? Because it lightweight, idiomatic, and composable router for building Go HTTP services. Especially, chi's router is based on `Radix trie`, so it'll handle the request as fast as possible if we have a lot of handlers in the future.
#### 3. We shouldn't use `GET` method for login because it have some problem it will save the user's information url, another user can find id and password in the browser's history,..So I have changed to `POST` method for /login
#### 4.1 Add config (with file - just easy for testing) - after load config file, it will overwrite the variable environment, so this project still abide by 12factor `https://12factor.net/` .P/s: Again the config file just save the infomation for quick run, when in production, use variable environment.
#### 4.2 Variable Environment in this project:
Key | Example value | Description
--- | --- | ---
`APP_ENV` | `dev` | represent the current app environment 
`SERVICE` | `todo` | name of the current service 
`JWT_KEY` | `wqGyEBBfPK9w3Lxw` | JWTKey 
`LOG_LEVEL` | `info` | represent level out the log 
`ADDRESS` | `:5050` | the address or port when the app will deploy
`LDB_PATH` | `./data.db` | the path of sqlite 

#### 5. Added logging for this project with json format, for easily tracing log (error), using zap here (uber).
#### 6. Remove unnecessary pointer variable (replaced by variable) to avoid memory allocate in the runtime to much.
-----
### Overview
This is a simple backend for a good old todo service, right now this service can handle login/list/create simple tasks.  
To make it run:
- `go run main.go`
- Import Postman collection from `docs` to check example

Candidates are invited to implement below requirements but the point is not to resolve everything in a perfect way but selective what you can do best in a limited time.  
Thus, there is no correct-or-perfect answer, your solutions are way for us to continue the discussion and collaboration.
 
### Requirements
Right now a user can add many task as they want, we want ability to limit N task per day.

Example: users are limited to create only 5 task only per day, if the daily limit is reached, return 4xx code to client and ignore the create request.
#### Backend requirements
- A nice README on how to run, what is missing, what else you want to improve but don't have enough time
- Fork this repo and show us your development progess by a PR.
- Write integration tests for this project
- Make this code DRY
- Write unit test for `services` layer
- Change from using `SQLite` to `Postgres` with `docker-compose`
- This project include many issues from code to DB strucutre, feel free to optimize them.
#### Frontend requirements
- A nice README on how to run, what is missing, what else you want to improve but don't have enough time
- https://github.com/manabie-com/mana-do
- Fork the above repo and show us your development progess by a PR.
#### Optional requirements
- Write unit test for `storages` layer
- Split `services` layer to `use case` and `transport` layer

### DB Schema
```sql
-- users definition

CREATE TABLE users (
	id TEXT NOT NULL,
	password TEXT NOT NULL,
	max_todo INTEGER DEFAULT 5 NOT NULL,
	CONSTRAINT users_PK PRIMARY KEY (id)
);

INSERT INTO users (id, password, max_todo) VALUES('firstUser', 'example', 5);

-- tasks definition

CREATE TABLE tasks (
	id TEXT NOT NULL,
	content TEXT NOT NULL,
	user_id TEXT NOT NULL,
    created_date TEXT NOT NULL,
	CONSTRAINT tasks_PK PRIMARY KEY (id),
	CONSTRAINT tasks_FK FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### Sequence diagram
![auth and create tasks request](https://github.com/manabie-com/togo/blob/master/docs/sequence.svg)
