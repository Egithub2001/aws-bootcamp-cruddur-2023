# Week 4 — Postgres and RDS

history of the command that helped me setup my RDS scripts 

  psql -Upostgres --host localhost
  
  psql -Upostgres cruddur <db/schema.sql 
  
  psql cruddur < db/schema.sql -h localhost -U postgres
  
  psql postgresql://postgres:password@localhost:5432/cruddur
  
  export CONNECTION_URL="postgresql://postgres:password@localhost:5432/cruddur"
  
  psql $CONNECTION_URL
  
  gp env export CONNECTION_URL="postgresql://postgres:password@localhost:5432/cruddur"
  
  gp env CONNECTION_URL="postgresql://postgres:password@localhost:5432/cruddur"
  
  chmod u+x bin/db*
  
  ./bin/db_drop 
  
  21  ./bin/db_create 
  
  22  ./bin/db_schema_load 
  
  23  ./bin/db_schema_load prod
  
  24  ./bin/db_schema_load 
  
  25  chmod +x bin/db_connect 
  
  26  ./bin/db_connect 
  
  27  ./bin/db_schema_load 
  
  28  ./bin/db_connect 
  
  29  ./bin/db_schema_load 
  
  30  ./bin/db_connect 
  
  31  ./bin/db_schema_load 
  
  32  ./bin/db_connect 
  
  33  chmod +x bin/db_s
  
  34  chmod +x bin/db_seed 
  
  35  ./bin/db_seed 
  
  36  ./bin/db_schema_load 
  
  37  ./bin/db_seed 
  
  38  ./bin/db_connect sir




My env setup
 export CONNECTION_URL="postgresql://postgres:password@localhost:5432/cruddur"
 
 gp env CONNECTION_URL="postgresql://postgres:password@localhost:5432/cruddur"

for production our RDS DB 

export PROD_CONNECTION_URL="postgresql://crudurroot:crudurpass123@cruddur-db-instance.chqhjemn1xyg.ca-central-1.rds.amazonaws.com:5432/cruddur"

gp env PROD_CONNECTION_URL="postgresql://crudurroot:crudurpass123@cruddur-db-instance.chqhjemn1xyg.ca-central-1.rds.amazonaws.com:5432/cruddur"

Command to fix the email verification/ temp password issue: ( needs to be done vi AWS CLI only)

    aws ec2 modify-security-group-rules \
    --group-id $DB_SG_ID \
    --security-group-rules "SecurityGroupRuleId=$DB_SG_RULE_ID,SecurityGroupRule=       {Description=GITPOD,IpProtocol=tcp,FromPort=5432,ToPort=5432,CidrIpv4=$GITPOD_IP/32}"
    
    
  for lambda to wor I need to add VPC and set the permision tab by creating a custome role : 
  
  <img width="848" alt="Screenshot 2023-03-18 at 11 57 21" src="https://user-images.githubusercontent.com/123549868/226116878-89efd1b3-e1cf-4fa7-a88f-79755d99c6af.png">

<img width="868" alt="Screenshot 2023-03-18 at 11 57 35" src="https://user-images.githubusercontent.com/123549868/226116951-5335b8f8-3ace-43a5-bd61-302ee3fe37bd.png">

  
