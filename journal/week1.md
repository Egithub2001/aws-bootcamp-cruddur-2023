# Week 1 — App Containerization
pretty much I followed Andrews's week 1 journal : 
https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-1/journal/week1.md

by adding the following lines to my docker composer file, I do not need to manually install npm i :
      
      # reason why I added the following line

#https://stackoverflow.com/questions/30043872/docker-compose-node-modules-not-present-in-a-volume-after-npm-install-succeeds
      - /frontend-react-js/node_modules
