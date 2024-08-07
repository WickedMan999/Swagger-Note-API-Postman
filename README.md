<img src="https://upload.wikimedia.org/wikipedia/commons/c/c2/Postman_%28software%29.png" alt="Postman Logo" style="max-width:100%;">


# Swagger Note

This project involves REST API testing on [Swagger Note API](https://practice.expandtesting.com/notes/api/api-docs/) using `Postman`. The Note API operates by creating a user account and facilitating login. After successful login, it generates a dynamic authentication token known as `x-auth-token`. Subsequently, authorization is required to access specific resources, such as creating, retrieving notes etc.
## Requirements

- [Git](https://git-scm.com/downloads),
- [Postman](https://www.postman.com/downloads/),
- [nodejs](https://nodejs.org/en/download), 
- [Newman Report Generator](https://www.npmjs.com/package/newman-reporter-htmlextra)

## What are tested?
- Covered all API endpoints for both `positive and negative scenarios` except two of them. (Reason explain on what are not tested section)
- All API requests are organized under `collection` folder properly so that they can be executed using a Postman's "Run collection" feature without the need to manually run each API one by one.
- Validate response data through different types of assertions, such as `response code, response time, response header, JSON schema validation and more`.
- Handle dynamic elements like the `token key`, which changes every time during a new login, by storing the captured token in an `environment variable` each time a new token is generated and updating it with the last one.
- Identified `6 minor and 2 critical bugs`.
- Generate HTML report using `Newman Report Generator`. The report is generated in the `newman` folder.

## What are not tested?
- 2 API endpoints `Forget password` and `Verify reset password token` are not tested because `Forget password` doesn't sent password reset link to the email despite email being correct. **NOTE**: This feature was working before but now its not working.

## Challanges faced during testing?
The first challenge I faced was during the `forgot password` process. The server sent a password reset link to an email address. However, during the `Verify reset password token` step, it was confusing to determine which token to insert for verification. I later realized that the token value was located at the end of the password reset link after the ‘/’ character.

The second challenge I faced was capturing the partial token value from a password reset link (for example, a password reset link looks like https://URL/token-key-to-reset). My plan was to parse the response, capture that partial value, and store it inside environment variables. So, I used the `Google Cloud Console` to generate a client ID and client secret and logged in with my Gmail ID on Postman, but that link was not visible in Postman. So, I completed the process by manually copying the token value from my Gmail and verifying the token to reset the password, which I failed to do automatically through Postman.

## How to Download and Setup the Project?

- Clone the project repository using below command or Alternatively, you can download the project as a ZIP file and extract it.
    ```
    git clone https://github.com/prabesh-mah/Swagger-Note-API-Postman
    ```
- After downloading and extracting the project files, launch Postman and create a new Workspace.
- Import the collection file into your newly created workspace. To do this, click on `Import`, which is located just below the `API Network` button on top LHS menu bar. A dialog box will open up where browse or drag and drop the file name `Swagger Note API.postman_collection.json`.
- Next, navigate to the 'Environment' section, which is located on the left-hand side (LHS) below the collection.
- Import the environment variable file in the same manner as you did for the collection file. The file you need to import is named `Swagger Note Variables.postman_environment.json`.
- Now, you're all set to start testing the API, Notice to set the environment variable as `QA-Environment` located on top RHS below maximize button. 

***NOTE***: Information such as `Name and Email` are automatically generated using Postman’s built-in keywords `$randomFullName and $randomEmail`. After hitting API request and getting the response these values are stored in `environment variables` through `assertion` for further validation. During validation, I used assertions to retrieve the same stored value that was stored in evironment variable and performed the validation.

After login, the `token key` is generated by the server-side as a response for authentication. The token value is dynamic, changing every time a new login occurs. This dynamic token value is also automatically handled by `assertions`, ensuring that the new token value is stored in the ‘token’ variable everytime a user logins. 

## How to run collection and generate report using newman cli?
```
newman run "Swagger Note API.postman_collection.json" -e "QA-Environment.postman_environment.json" -r htmlextra
```

<img src="screenshot\newman-result.png" alt="Newman Report" style="max-width:100%;">


## Load Testing
The results for load testing are coming soon as I’m planning to run performance tests from Postman to identify bottlenecks and determine which requests are experiencing issues.