# The Task

Create a micro-service to submit and manage a credit application  using spring boot. Application decision needs to be obtained from an external service which can be mocked using wiremock. 

## Requirements

- Customer should be able to 

  - apply for credit and get a decision (accepted, rejected)
  - get the application status
  - cancel the application
  - amend the application with updated credit amount. On amend, the decisioning logic should apply and return a status. 

- Decisioning service can be a wiremock service and should send `accepted` status if credit amount is between 250 and 1500. All other amounts should be `rejected`. 
- Application can be stored in a local/in-memory database of your choice

Please provide a solution that best showcases your OO design abilities and is representative of how you would work.
Please document the decisions you made while solving this task (taking shortcuts is okay if they are justified and explained).
Please make a note of what you skipped or didn't implement and why.

## Evaluation

The main evaluation criteria will be:

- OO design/principles
- API documentation
- REST principles
- Integration test and unit tests
- Error handling

## Submitting your solution

Please don't fork the repo, as your solution will be public.

Either:

Zip up your solution (omitting the vendor folder)
After committing your changes, create a git bundle from your local repo with:
	git bundle create solution.bundle master
	
Submit your solution via email
