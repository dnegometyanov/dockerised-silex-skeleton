The Docker setup with Silex framework skeleton php application using PHP7-FPM and Nginx based on two repositories:

    https://github.com/mikechernev/dockerised-php

    https://github.com/vesparny/silex-simple-rest

**This project wants to be a starting point to writing scalable and maintainable REST api with Silex PHP micro-framework**

## Instructions
1. Checkout the repository
2. Run `docker-compose up --build` (further, if no changes were made to env, you can use just `docker-compose up`, also there is an option `-d` for background mode)
3. Run `docker ps`, it  should display two running containers, something like

    _fbd8e14b7db1        dockerisedsilexskeleton_php   "docker-php-entryp..."   3 hours ago         Up 3 hours                  9000/tcp                           dockerisedsilexskeleton_php_1_

    _9eb0cb0f6e7d        nginx:latest                  "nginx -g 'daemon ..."   3 hours ago         Up 3 hours                  443/tcp, 0.0.0.0:8080->80/tcp      dockerisedsilexskeleton_web_13_

4. Using the container name of php container, for example in the case upper it is `dockerisedsilexskeleton_php_1`

    Run `docker exec -it dockerisedsilexskeleton_php_1 composer install`

    This should install composer dependencies for the project.

4. Import Sqlite3 db dump:

```
    docker exec -i dockerisedsilexskeleton_php_1 bash <<'EOF'
    sqlite3 app.db < /var/www/silex/resources/sql/schema.sql
    exit
    EOF
```
 
5. `curl http://localhost:8080/api/v1/notes -H 'Content-Type: application/json' -w "\n"`

That's it! You have your local Silex skeleton setup using Docker
    
#### What you will get
The api will respond to

	GET  ->   http://localhost:8080/api/v1/notes
    GET  ->   http://localhost:8080/api/v1/notes/{id}
	POST ->   http://localhost:8080/api/v1/notes
	PUT ->   http://localhost:8080/api/v1/notes/{id}
	DELETE -> http://localhost:8080/api/v1/notes/{id}

Your request should have 'Content-Type: application/json' header.
Your api is CORS compliant out of the box, so it's capable of cross-domain communication.

Try with curl:
	
	#GET (collection)
	curl http://localhost:8080/api/v1/notes -H 'Content-Type: application/json' -w "\n"
	
	#GET (single item with id 1)
    curl http://localhost:8080/api/v1/notes/1 -H 'Content-Type: application/json' -w "\n"

	#POST (insert)
	curl -X POST http://localhost:8080/api/v1/notes -d '{"note":"Hello World!"}' -H 'Content-Type: application/json' -w "\n"

	#PUT (update)
	curl -X PUT http://localhost:8080/api/v1/notes/1 -d '{"note":"Uhauuuuuuu!"}' -H 'Content-Type: application/json' -w "\n"

	#DELETE
	curl -X DELETE http://localhost:8080/api/v1/notes/1 -H 'Content-Type: application/json' -w "\n"
	
	
#### Run tests
Some tests were written, and all CRUD operations are fully tested :)

From the root folder run the following command to run tests.
    
    vendor/bin/phpunit 
