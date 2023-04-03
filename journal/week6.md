# Week 6 — Deploying Containers


I had the following issue next time trying to continue setting up my ECR:


"failed to solve: 533378368499.dkr.ecr.ca-central-1.amazonaws.com/cruddur-python:3.10-slim-buster: pulling from host 533378368499.dkr.ecr.ca-central-1.amazonaws.com failed with status code [manifests 3.10-slim-buster]: 401 Unauthorized"

and I realized becaaue I forgot to login before that:

aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin "$AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com"

also for flask docekr file, make sure you cd to backend-flask directory before running this: 

docker build -t backend-flask .

for Passing Senstive Data to Task Defintion:

first I export honeycob header in gipod terminal and then did teh following:

 28  export OTEL_EXPORTER_OTLP_HEADERS="x-honeycomb-team=${HONEYCOMB_API_KEY}"
 
 make sure is there:
 
 echo $OTEL_EXPORTER_OTLP_HEADERS
 
 and then run these:
 
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/AWS_ACCESS_KEY_ID" --value $AWS_ACCESS_KEY_ID
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/AWS_SECRET_ACCESS_KEY" --value $AWS_SECRET_ACCESS_KEY
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/CONNECTION_URL" --value $PROD_CONNECTION_URL
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/ROLLBAR_ACCESS_TOKEN" --value $ROLLBAR_ACCESS_TOKEN
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/OTEL_EXPORTER_OTLP_HEADERS" --value "x-honeycomb-team=$HONEYCOMB_API_KEY"

the above command will create following parameter in system manager:

<img width="1106" alt="Screenshot 2023-04-03 at 04 38 52" src="https://user-images.githubusercontent.com/123549868/229457121-24b839e4-92c6-411b-943c-1f8920a76883.png">

