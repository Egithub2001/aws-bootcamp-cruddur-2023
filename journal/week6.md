# Week 6 — Deploying Containers


I had the following issue next time trying to continue setting up my ECR:

```sh
"failed to solve: 533378368499.dkr.ecr.ca-central-1.amazonaws.com/cruddur-python:3.10-slim-buster: pulling from host 533378368499.dkr.ecr.ca-central-1.amazonaws.com failed with status code [manifests 3.10-slim-buster]: 401 Unauthorized"
```

and I realized becaaue I forgot to login before that:

```sh
aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin "$AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com"
```
also for flask docekr file, make sure you cd to backend-flask directory before running this: 

```sh
docker build -t backend-flask .
```

for Passing Senstive Data to Task Defintion:

first I export honeycob header in gipod terminal and then did teh following:

```sh

  export OTEL_EXPORTER_OTLP_HEADERS="x-honeycomb-team=${HONEYCOMB_API_KEY}"
 
 ```
 make sure is there:
 
 ```sh
 echo $OTEL_EXPORTER_OTLP_HEADERS
 
 ```
 and then run these:
 
 ```sh
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/AWS_ACCESS_KEY_ID" --value $AWS_ACCESS_KEY_ID
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/AWS_SECRET_ACCESS_KEY" --value $AWS_SECRET_ACCESS_KEY
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/CONNECTION_URL" --value $PROD_CONNECTION_URL
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/ROLLBAR_ACCESS_TOKEN" --value $ROLLBAR_ACCESS_TOKEN
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/OTEL_EXPORTER_OTLP_HEADERS" --value "x-honeycomb-team=$HONEYCOMB_API_KEY"

```

the above command will create following parameter in system manager:

<img width="1106" alt="Screenshot 2023-04-03 at 04 38 52" src="https://user-images.githubusercontent.com/123549868/229457121-24b839e4-92c6-411b-943c-1f8920a76883.png">

the right approach is to create the following not to cobmine them in one policy file:


```sh

service-assume-role-execution-policy.json
service-execution-policy.json 
```

and then below comman dworked, not with \ and multiple lines, for some wierd reason!

```sh
aws iam create-role --role-name CruddurServiceExecutionPolicy  --assume-role-policy-document "file://aws/policies/service-assume-role-execution-policy.json"
```

for creating role and policy with CLI:  (Andrew had to finally do it with aws console, but I figured it with aws cli):
had to update "service-execution-policy.json" with this line:       "Resource": "arn:aws:ssm:ca-central-1:533378368499:parameter/cruddur/backend-flask/*"


```sh
   55  aws iam create-role --role-name CruddurServiceExecutionRole --assume-role-policy-document file://aws/policies/service-execution-policy.json
   
   aws iam put-role-policy --role-name CruddurServiceExecutionRole --policy-name CruddurServiceExecutionPolicy --policy-document file://aws/policies/service-execution-policy.json
   ```
  and then I run following on gitpod terminal:
  
  ```sh 
  aws iam create-role \
    --role-name CruddurTaskRole \
    --assume-role-policy-document "{
  \"Version\":\"2012-10-17\",
  \"Statement\":[{
    \"Action\":[\"sts:AssumeRole\"],
    \"Effect\":\"Allow\",
    \"Principal\":{
      \"Service\":[\"ecs-tasks.amazonaws.com\"]
    }
  }]
}"

```

and then run below to use session manager:

```sh
aws iam put-role-policy \
  --policy-name SSMAccessPolicy \
  --role-name CruddurTaskRole \
  --policy-document "{
  \"Version\":\"2012-10-17\",
  \"Statement\":[{
    \"Action\":[
      \"ssmmessages:CreateControlChannel\",
      \"ssmmessages:CreateDataChannel\",
      \"ssmmessages:OpenControlChannel\",
      \"ssmmessages:OpenDataChannel\"
    ],
    \"Effect\":\"Allow\",
    \"Resource\":\"*\"
  }]
}
"
```
  
the above command already created the rule and one policyepolicy, now we are attaching more policies.

```sh 
aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/CloudWatchFullAccess --role-name CruddurTaskRole
```

and to be able to write to teh daemon: 
```sh
aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/AWSXRayDaemonWriteAccess --role-name CruddurTaskRole
```

and then create aws/task-definitions and created related json files for backend task, had to update the task with my own aws account number and made sure keys and paramater are the same as docker-compose file.
then run the following command:
```sh 
aws ecs register-task-definition --cli-input-json file://aws/task-definitions/backend-flask.json

```
<img width="1294" alt="Screenshot 2023-04-03 at 06 06 29" src="https://user-images.githubusercontent.com/123549868/229479117-aa725d6f-2c16-4911-bec1-95ff5147b0ac.png">


had to add more permison to CruddurServiceExecutionPolicy


<img width="1033" alt="Screenshot 2023-04-04 at 21 59 35" src="https://user-images.githubusercontent.com/123549868/229961691-45f8a826-3655-42f7-8913-782d7df050e4.png">


had to install session manager 

```sh
curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"

sudo dpkg -i session-manager-plugin.deb

```
and then connect to ecs:

```sh
aws ecs execute-command  --region $AWS_DEFAULT_REGION --cluster cruddur --task fc1a83bb54f4452b82bf527286a336fe --container backend-flask --command "/bin/bash" --interactive
```



Create Repo

aws ecr create-repository \
  --repository-name backend-flask \
  --image-tag-mutability MUTABLE

Set URL

export ECR_BACKEND_FLASK_URL="$AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/backend-flask"
echo $ECR_BACKEND_FLASK_URL

Build Image

docker build -t backend-flask .

Tag Image

docker tag backend-flask:latest $ECR_BACKEND_FLASK_URL:latest
