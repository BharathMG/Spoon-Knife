MTurk API
=========

> SetUp :

* [config/keys.js] contains the AWS Access Key and Access Secret Credentials
* Please follow the instructions in http://docs.aws.amazon.com/AWSMechTurk/latest/AWSMechanicalTurkGettingStartedGuide/SetUp.html to set up your MTurk AWS Account.
* And get your Access Key and Access Secret and update in [config/keys.js].
* Install postgresql on your machine.
* I used **homebrew** to install postgresql. First install homebrew and then,
  ```sh
  $ brew update
  $ brew doctor
  $ brew install postgresql

  ```
  
* Please follow these instructions http://www.moncefbelyamani.com/how-to-install-postgresql-on-a-mac-with-homebrew-and-lunchy/ to install postgresql if you are not successfull with previous setup.
* After installing postgresql, you need to start the postgresql server and create database,
 ```sh
 $ pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
 $ initdb
 $ createdb awsturk
 ```
 
 * Now in **config/settings.js**, replace the database username and password with your credentials.

```sh
   npm install
   node services/setup.js (Bootstrap Database)
   node app
```

This will start the localhost server at Port 3000.

>Postman

 * Install Postman REST Client Chrome Extension.
 * Import **postman.json** from root folder to Postman
 * Before making API Calls, you will need to login and get Access Tokens for all the Users.
 * Make login requests for three users (**User, Admin and Global Admin**) and copy the **Access Tokens** for all the three users.
 * Login Requests Name in the Postman Collection are,
**OAuth2 Login User, OAuth2 Login Admin User, OAuth2 Login Global Admin User**.
 * First make these three requests before making any API Call, as you will need to send the Access Token with all the API Calls.
 * In Postman requests JSON, before making request, each time replace the following

```sh 
"Bearer <user_token>" with "Bearer <access_token for the user@topcoder.com>"
"Bearer <admin_user_token>" with "Bearer <access_token for the admin@topcoder.com>"
"Bearer <global_admin_user_access_token>" with "Bearer <access_token for the global_admin@topcoder.com>"
```
and hit the request.
 * For user@topcoder.com = User Role
 , admin@topcoder.com = Admin Role and
   global_admin@topcoder.com = Global Admin Role
 * If you get, **"Access Token is expired"** message in any API Call, make the Login Request for that user again, and replace the new access token with the expired token.
 * Example Authorization Header,
```sh
Authorization: Bearer 191855af4737217d1c77dbe64ac4fbbc262500da
```

[config/keys.js]:config/keys.js

>API Calls

* #### Create HIT
   http://docs.aws.amazon.com/AWSMechTurk/latest/AWSMturkAPI/ApiReference_CreateHITOperation.html
   Route : **/domain/:domainId/hits**  
   Method: **POST**

  Params: 
  
      'Title': "Hit Title",
	  'Description': "Hit Description",
      "Question": "<QuestionForm xmlns='http://mechanicalturk.amazonaws.com/AWSMechanicalTurkDataSchemas/2005-10-01/QuestionForm.xsd'><Question><QuestionIdentifier>q1</QuestionIdentifier><QuestionContent><Text>Who invented the car? Hit Two? </Text></QuestionContent><AnswerSpecification><FreeTextAnswer></FreeTextAnswer></AnswerSpecification></Question></QuestionForm>",
      'LifetimeInSeconds': 60 * 20,  // How long should the assignment last?
      'MaxAssignments': 10,          // How many responses do you want?
      'Reward': {Amount: 1.0, CurrencyCode: "USD"},
      'AssignmentDurationInSeconds': 3600
     
  Sample Response:
  
      { 
        "mturk_hit_id": "3R15W654VDU2Y51QJIROSRCHYJYLQM",
        "mturk_title": "Hit Title",
        "task_id": 5,
        "domain_id": 1,
        "created_by": 3,
        "created_date": "2014-10-10T20:31:39.719Z",
        "modified_by": 3,
        "modified_date": "2014-10-10T20:31:39.719Z"
      }
      
* #### Extend HIT
   http://docs.aws.amazon.com/AWSMechTurk/latest/AWSMturkAPI/ApiReference_ExtendHITOperation.html
  Route: **/hit/:mturkHITId**.
  
  Method: **PUT**
 
  Params: 

      'MaxAssignmentsIncrement' : 10
      'ExpirationIncrementInSeconds' : 3600 
      
   Sample Response:
        
      204 Status Code if Success
      Error message if Failure
      
* #### Disable HIT
 http://docs.aws.amazon.com/AWSMechTurk/latest/AWSMturkAPI/ApiReference_DisableHITOperation.html
 Route: **/hit/:mturkHITId**.

 Method: **DELETE**
 
 Sample Response:
        
      204 Status Code if Success
      Error message if Failure
 
* #### Get HIT
  http://docs.amazonwebservices.com/AWSMechTurk/latest/AWSMturkAPI/ApiReference_GetHITOperation.html
  Route: **/hit/:mturkHITId**

  Method: **GET**
  
  Sample Response:
     
      {
        "Request": {
            "IsValid": "True"
        },
        "HITId": "3R15W654VDU2Y51QJIROSRCHYJYLQM",
        "HITTypeId": "3LGCS0G1JAMPTOUWF6072KGMLP6KK6",
        "HITGroupId": "3IR329NZ7YHBRGFZEDZXSRNHHIWK2J",
        "CreationTime": "2014-10-10T20:31:40Z",
        "Title": "Hit Title",
        "Description": "Hit Title Description",
        "Question": "<QuestionForm xmlns='http://mechanicalturk.amazonaws.com/AWSMechanicalTurkDataSchemas/2005-10-01/QuestionForm.xsd'><Question><QuestionIdentifier>q1</QuestionIdentifier><QuestionContent><Text>Who invented the umbrella?</Text></QuestionContent><AnswerSpecification><FreeTextAnswer></FreeTextAnswer></AnswerSpecification></Question></QuestionForm>",
        "HITStatus": "Assignable",
        "MaxAssignments": "110",
        "Reward": {
            "Amount": "1.00",
            "CurrencyCode": "USD",
            "FormattedPrice": "$1.00"
        },
        "AutoApprovalDelayInSeconds": "2592000",
        "Expiration": "2014-10-10T20:51:40Z",
        "AssignmentDurationInSeconds": "3600",
        "HITReviewStatus": "NotReviewed",
        "task_id": 5,
        "domain_id": 1,
        "created_by": 3,
        "created_date": "2014-10-10T18:30:00.000Z",
        "modified_by": 3,
        "modified_date": "2014-10-10T20:33:06.220Z"
        }

* #### Get All HITs
 Route: **/domain/:domainId/hits**

 Method: **GET**
 
 Sample Response:
 
      [
        {
            "mturk_hit_id": "3O2Y2UIUCQVUNATZK24T1SVO2H6FK5",
            "mturk_title": "Hit Two Global Admin",
            "task_id": 2,
            "domain_id": 2,
            "created_by": 3,
            "created_date": "2014-10-09T18:30:00.000Z",
            "modified_by": 3,
            "modified_date": "2014-10-09T18:30:00.000Z"
        },
        {
            "mturk_hit_id": "3IZVJEBJ6ALTTCZAJA4ZU16ODRI6ZH",
            "mturk_title": "Hit Four",
            "task_id": 3,
            "domain_id": 2,
            "created_by": 3,
            "created_date": "2014-10-09T18:30:00.000Z",
            "modified_by": 3,
            "modified_date": "2014-10-09T18:30:00.000Z"
        }
      ]
 
  

  
