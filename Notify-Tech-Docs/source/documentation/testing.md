# Testing

All testing takes place in the production environment. There is no test environment for GOV.UK Notify.

Testing is divided into smoke testing and non-smoke testing.

## Smoke testing

If you need to [smoke test](https://www.gov.uk/service-manual/technology/deploying-software-regularly#using-smoke-tests-after-you-deploy) your integration with GOV.UK Notify on a regular basis, you  must use the smoke test phone numbers and email addresses below.

|Phone number|
|:---|
|07700900111|
|07700900222|
|07700900333|

|Email address|
|:---|
|simulate-delivered@notifications.service.gov.uk|
|simulate-delivered-2@notifications.service.gov.uk|
|simulate-delivered-3@notifications.service.gov.uk|

The smoke test phone numbers and email addresses will validate the request and stimulate a successful response, but won’t send a real message, produce a delivery receipt or persist the notification to the database.

You can use these smoke test numbers and addresses with any [type of API key](/#types-of-api-keys).

You can smoke test all GOV.UK Notify API client functions except:

- Get the status of one message
- Get the status of all messages

You cannot use the smoke test phone numbers or email address with these functions because they return a mock notification_ID. If you need to test these functions, use a test API key and any other phone number or email.

## Non-smoke testing

If you need to do non-smoke testing, such as performance or integration testing _QP: is this right?_, you must use a test API key. You can use any non-smoke testing phone numbers or email addresses. You don’t need a specific GOV.UK Notify testing account.



The method of testing depends on the type of test. Non-smoke testing such as performance or integration testing  

General testing




_QP: What exactly are we testing?_

_QP: What types of different tests are there? Smoke tests. Functional testing? Other testing? PERFORMANCE (SPEED OF APPLICATION WITH NOTIFY INTEGRATION I.E. TIME TAKEN TO ACCOMPLISH A TASK) TESTING, INTEGRATION TESTING (DOES THE INTEGRATION WORK, PARTS SLOT TOGETHER, KEYS VALID ETC)_

Testing your service  GOV.UK Notify takes place in the production environment and uses specific test keys.

no different environments, use prod env, but use a range of keys, magic numbers etc



_QP: Should you use these numbers and addresses when you first set up the client?_

_QP: Should we provide any guidance on how regularly users should smoke test? NO_

_QP: Are the phone number / email addresses in pairs? NO_
