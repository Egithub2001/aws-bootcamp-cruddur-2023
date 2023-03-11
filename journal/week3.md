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


