# csat
A proof of concept customer satisfaction meter

Customer satisfaction is important, this application provides a simple method of capturing customer satisfaction ratings over time

## Limitations

This application is a proof of concept, it is **not** production ready.
A non-exhaustive list of known limitations:
* Ratings can be submitted multiple times per person within a period allowing the results to be intentionally or unintentionally skewed
* No security whatsoever - anonymous users can easily delete or alter all data

## Dependencies

1. MongoDB

## Quick start guide

### Add a team

```
 curl -H "Content-Type: application/json" -X POST -d '{"teamId": "<team id>", "teamName": "Team Name", "platformName": "Platform Name", "domainName": "Domain Name"}' http://localhost:8080/team
```

### Add customers

```
curl -H "Content-Type: application/json" -X POST -d '{"teamId": "<team id>", "email": "user.email@big.co"}' http://localhost:8080/customer
```

### Update customers
```
curl -H "Content-Type: application/json" -X PUT -d '{"teamId": "<team id>", "email": "user.email@big.co"}' http://localhost/customer/{itemid}
```
### View customers

<http://localhost:8080/customer>

### Delete a team member

```
curl -X DELETE http://localhost:8080/customer/{id}
```

### View customer satisfaction trends for the last 12 months

<http://localhost:8080>


---

## Developer guide

### Compile

```
mvn clean install
```

### Run in development

*Might be **-Drun.arguments** - see: https://stackoverflow.com/questions/23316843/get-command-line-arguments-from-spring-bootrun*

```
mvn spring-boot:run -Dspring-boot.run.arguments="--spring.data.mongodb.host=<mongo host>,--spring.data.mongodb.port=<mongo port>,--spring-data.mongodb.database=<mongo database>"
```

### Create Docker images

```
mvn dockerfile:build dockerfile:tag
```

### Run app on Docker

*See https://github.com/docker-library/openjdk/issues/135 as to why spring.boot.mongodb.. env vars don't work*

```
docker stop csat_app
docker rm csat_app
docker run --name csat_app -d -p 8080:8080 --network mongonetwork -e spring_data_mongodb_host=<mongo host> -e spring_data_mongodb_port=<mongo port> -e spring_data_mongodb_database=<mongo database> team/csat:0.0.1-SNAPSHOT
```

