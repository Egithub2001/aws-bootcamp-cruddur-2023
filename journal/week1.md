# Week 1 — App Containerization
pretty much I followed Andrews's week 1 journal : 
https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-1/journal/week1.md

by adding the following lines to my docker composer file, I do not need to manually install npm i :
      
      # reason why I added the following line

#https://stackoverflow.com/questions/30043872/docker-compose-node-modules-not-present-in-a-volume-after-npm-install-succeeds
     
     - /frontend-react-js/node_modules


Homework challenge: 
I worked on all he homework challenges and I encountered a few issues and learned new things from each tasks as follows:

- I had an issue with running the dockerfile CMD as an external script, I found out it was about the permission of the script on my local directory which I fixed the issue and it worked

- I created a docker hub account and pushed separate frontend and backend images to my repositories, but not sure if we can push both in the same images, not sure if this makes sense at all

- By applying multi-stage building for a Dockerfile build, I learned multi staging approach separates the build and runtime environments, allowing us to install the Python dependencies in a clean environment and avoid including unnecessary build tools in the final image. It also reduces the image size, as the builder stage is discarded after the dependencies are installed. 
My bakconed images changed from 129 M to 125 M 
And frontend docker changed from 1.19 g to 122M

- I added healthchek for both backend and frontend, and I realised the health check lines needs to be added to composer file.
- 
- <img width="1028" alt="Screenshot 2023-02-24 at 19 22 47" src="https://user-images.githubusercontent.com/123549868/221324708-2bd89ecf-4ca5-4015-904e-9e3ce2517998.png">
