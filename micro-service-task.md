# The Task

Create a java spring boot micro-service to submit a credit application and get a decision. Application decision needs to be obtained from an external service which can be configured using wiremock. 

## Requirements

- Customer should be able to 
  - apply for credit and get a decision (accepted, rejected)
  - get the application status
- Application can be stored in in-memory database (H2)
- Application may contain - customer name, application reference, loan value, decision status
- Decisioning service  - refer to section `decisioning service`. 

Please provide a solution that demonstrats your abilities creating a micro-service using spring boot.
Please document the decisions you made while solving this task (taking shortcuts is okay if they are justified and explained).
Please make a note of what you skipped or didn't implement and why.

## Evaluation

The main evaluation criteria will be:

- API documentation - openapi3 or swagger
- REST principles, spring boot features
- Integration and unit tests

## Submitting your solution

Please don't fork the repo, as your solution will be public.

Either:

Zip up your solution (omitting the vendor folder)
After committing your changes, create a git bundle from your local repo with:
	git bundle create solution.bundle main
	
Submit your solution via email

## Decisioning service

Service will be running on port `9999` and will return a decision outcome as json response

If the loan value is between 500 and 1000 - status will be `accepted`, all other cases `rejected`

To set-up wiremock for decisioning service 

1) Install and run wiremock locally - http://wiremock.org/docs/running-standalone/ 
	
```
java -jar  wiremock-standalone-2.27.2.jar --port 9999 &
```

2) Configure stubs
	
```
curl --location --request POST 'http://localhost:9999/__admin/mappings/new' \
--header 'Content-Type: application/json' \
--data-raw '{
    "request": {
        "urlPath": "/decision",
        "method": "GET",
        "headers": {
            "Content-Type": {
                "equalTo": "application/json"
            }
        },
        "queryParameters": {
            "value": {
                "matches": "^([5-9][0-9][0-9]|1000)$"
            }
        }
    },
    "response": {
        "status": 200,
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "{ \"outcome\": \"accepted\" }"
    }
}'

curl --location --request POST 'http://localhost:9999/__admin/mappings/new' \
--header 'Content-Type: application/json' \
--data-raw '{
    "request": {
        "urlPath": "/decision",
        "method": "GET",
        "headers": {
            "Content-Type": {
                "equalTo": "application/json"
            }
        },
        "queryParameters": {
            "value": {
                "doesNotMatch": "^([5-9][0-9][0-9]|1000)$"
            }
        }
    },
    "response": {
        "status": 200,
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "{ \"outcome\": \"rejected\" }"
    }
}'

```

3) To test the decisioning service
Request - 
```
curl --location --request GET 'http://localhost:9999/decision?value=1000' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json'
```
Response - 
```
{"outcome": "accepted"}
```
