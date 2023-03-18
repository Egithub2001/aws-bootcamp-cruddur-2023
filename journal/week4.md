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

