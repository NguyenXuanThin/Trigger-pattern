# Trigger handler pattern in Salesforce
## Advantage of Trigger Handler pattern

* Apex Class that handles trigger logic
* Allows code to be called from other code or tests
* Uses specific Trigger contexts and Trigger variables for routing
* Keep Triggers Simple
* Allow for greater flexibility
* Make code reusable
* Unit tests are much easier

![screenshot](https://github.com/NguyenXuanThin/Trigger-pattern/blob/main/image.png)

## Here are a few best practices for triggers in Salesforce:

* Bulkify your Code: Always write triggers that can handle multiple records at a time to respect Salesforce’s governor limits.
* Avoid Recursive Triggers: Ensure your trigger doesn’t re-fire itself, leading to an infinite loop.
* One Trigger per Object: To keep better control of the order of operations, it’s recommended to have only one trigger per object.
* Keep Business Logic in Helper Classes: Instead of writing all the business logic inside the trigger, write the logic in a separate class and call this class from the trigger.
* Test Coverage: Always aim for a minimum of 75% test coverage to ensure the trigger works as expected.
* Error Handling: Include proper error handling to display meaningful error messages to the end user.
