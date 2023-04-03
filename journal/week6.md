# Week 6 — Deploying Containers


I had the following issue next time trying to continue setting up my ECR:


"failed to solve: 533378368499.dkr.ecr.ca-central-1.amazonaws.com/cruddur-python:3.10-slim-buster: pulling from host 533378368499.dkr.ecr.ca-central-1.amazonaws.com failed with status code [manifests 3.10-slim-buster]: 401 Unauthorized"

and I realized becaaue I forgot to login before that:

aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin "$AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com"

also for flask docekr file, make sure you cd to backend-flask directory before running this: 

docker build -t backend-flask .
