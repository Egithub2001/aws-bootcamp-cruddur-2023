# Week 3 — Decentralized Authentication

setup AWS cognito, need to make sure the following settings are configured.
for Authentication providers section go with "cognito user pool" no need federation for now
sign in option: go with one like "email" 
No MFA ( more cost if I choose)
Delivery method "Email only" 
for configure sign up experience, everything as default just add perfered username and name to additional required attributes.
for Configure message delivery, go with "send email with Cognito" 
choose a name for "User pool name" cuudr-user-pool-name
do not check "use the Cognito Hosted UI" Andrew mentioned it is not great or there is soem challenges and issue with this option.
for initial app client go with "public client" 
enter "app cleint name" : Cluddr 

![my-cognito-user-pool](https://user-images.githubusercontent.com/123549868/224494249-86f20a2b-0d76-48e4-af91-19b7809d315e.png)


When adding a new user to cognito user pool. th default password needs to be changed but there was not any option to fix it, so we tried with AWS CLI instead as follow:

   aws cognito-idp admin-set-user-password --usernam emma2023 --password Test123! --user-pool-id ca-central-1_glu56GPlw  --permanent

# React code notes: 
previously before having cognito we used cookies library but for using cognito we replaced that library with amplify as follow: 

    import { Auth } from 'aws-amplify';
    //import Cookies from 'js-cookie'
 I made the above changes for all the signin, sign up, confirmation js files under frontend/sr/pages 
 also changed setErrorsCognito to setErrors, to make all consistent. 
 
 # troubleshooting notes:
  user console log to show some varaibles in console and by going to browser>inspect, you will see what is going on when you encounter an unexpected issue.
  
  # extra notes:
  obviosuly need to update docker compse file to add reuired env vraible related to react under frontend react as follows:
  
  REACT_APP_AWS_PROJECT_REGION: "ca-central-1"
      #REACT_APP_AWS_COGNITO_IDENTITY_POOL_ID: 
      REACT_APP_AWS_COGNITO_REGION: "ca-central-1"
      REACT_APP_AWS_USER_POOLS_ID: "cfrommy aws pool id"
      REACT_APP_CLIENT_ID: "from my aws client id"
  
  ca-central-1 is my default AWs regin. 
   
 You can see preferred name : emma is now showing in teh side bar blow "Crud" button.
 
![my-frontend](https://user-images.githubusercontent.com/123549868/224494817-203f9e48-0c97-4a3e-acf7-1d291cd69735.png)
