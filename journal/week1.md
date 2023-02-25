# Week 1 — App Containerization

pretty much I followed Andrews's week 1 journal : 
https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-1/journal/week1.md

by adding the following lines to my docker composer file, I do not need to manually install npm i :
      
      # reason why I added the following line

#https://stackoverflow.com/questions/30043872/docker-compose-node-modules-not-present-in-a-volume-after-npm-install-succeeds
     
     - /frontend-react-js/node_modules


## Homework challenge: 
I worked on all he homework challenges and I encountered a few issues and learned new things from each tasks as follows:

- I had an issue with running the dockerfile CMD as an external script, I found out it was about the permission of the script on my local directory which I fixed the issue and it worked

- I created a docker hub account and pushed separate frontend and backend images to my repositories, but not sure if we can push both in the same images, not sure if this makes sense at all

 
- <img width="1028" alt="Screenshot 2023-02-24 at 19 22 47" src="https://user-images.githubusercontent.com/123549868/221324708-2bd89ecf-4ca5-4015-904e-9e3ce2517998.png">

- By applying multi-stage building for a Dockerfile build, I learned multi staging approach separates the build and runtime environments, allowing us to install the Python dependencies in a clean environment and avoid including unnecessary build tools in the final image. It also reduces the image size, as the builder stage is discarded after the dependencies are installed. 


### how I used multisatging for backend and frontend dockers: 

#### Backend:

<img width="532" alt="Screenshot 2023-02-24 at 19 25 38" src="https://user-images.githubusercontent.com/123549868/221324881-8db4f25f-0f03-434b-8974-7bd291e51849.png">

In this Dockerfile, the build stage is named "builder" and the runtime stage is named "runtime". In the build stage, we copy the requirements file, install the dependencies using pip, and copy the rest of the application code. We also set the PATH environment variable to include the user's local bin directory, so that any scripts installed using pip can be run from the command line.

In the runtime stage, we use a fresh Python image and copy only the necessary files from the builder stage, including the Python packages installed in the user's local directory. We also set the FLASK_ENV environment variable and expose the specified port. Finally, we copy the startup script and set it as the entrypoint.

This approach separates the build and runtime environments, allowing us to install the Python dependencies in a clean environment and avoid including unnecessary build tools in the final image. It also reduces the image size, as the builder stage is discarded after the dependencies are installed.

#### Frontend: 
<img width="500" alt="Screenshot 2023-02-24 at 19 26 27" src="https://user-images.githubusercontent.com/123549868/221324927-099d5184-1592-4488-8aeb-7821e82e239a.png">

In this Dockerfile, the build stage is named "builder" and the runtime stage is named "runtime". In the build stage, we first copy the package.json and package-lock.json files and install the dependencies using npm install. We then copy the rest of the application code and build the production-ready assets using npm run build.

In the runtime stage, we use a fresh Node.js image and copy only the necessary files from the builder stage, including the built assets and the startup script. We also set the PORT environment variable and expose the specified port. Finally, we set the startup script as the entrypoint.

This approach separates the build and runtime environments, allowing us to install the dependencies and build the assets in a clean environment and avoid including unnecessary build tools in the final image. It also reduces the image size, as the builder stage is discarded after the assets are built. The use of the Alpine image for the runtime stage further reduces the image size by using a smaller base image.


My bakconed images changed from 129 M to 125 M 
And frontend docker changed from 1.19 g to 122M

# I added healthchek for both backend and frontend, and I realised the health check lines needs to be added to composer file:

- defined a service called backend that is built from the Dockerfile using the runtime target. We have also defined a healthcheck configuration option that will run every 30 seconds. The test option specifies the command to run to check the health of the container. In this case, the command is curl -f http://localhost:${PORT}/healthcheck, which will access the /healthcheck endpoint to verify that the application is running correctly.

We have also specified the interval, timeout, and retries options. The interval option specifies how often the healthcheck command should be run, the timeout option specifies how long the command has to complete before it is considered failed, and the retries option specifies how many times the command should be retried before the container is considered unhealthy.

If the healthcheck command returns a non-zero exit code or exceeds the specified timeout, the container will be considered unhealthy and will be restarted if the restart policy is set to on-failure. You can also use the depends_on option to ensure that the healthcheck is run after any necessary dependencies have started up.

Note that in order for the healthcheck to work, your Flask application needs to have an endpoint that returns a response indicating that the application is healthy. You can define such an endpoint in your application code and map it to the /healthcheck path in your Flask routes.

The healthcheck configuration is actually defined in the Docker Compose file, not in the Dockerfile.

So you would need to create a new or modify an existing Docker Compose file for your application and add the healthcheck configuration to the service you want to monitor.

## backend healthcheck : 

-<img width="561" alt="Screenshot 2023-02-24 at 19 32 34" src="https://user-images.githubusercontent.com/123549868/221325316-5b2694e5-b454-45dc-b90b-b049a807afe5.png">

## Frontend healthcheck:**

<img width="586" alt="Screenshot 2023-02-24 at 19 33 16" src="https://user-images.githubusercontent.com/123549868/221325365-6b0319b8-6390-4d7e-86e5-b73c879aaeae.png">



## some best practices for Dockerfiles:

Use official base images: Instead of creating your own base image, use an official base image from Docker Hub or a trusted registry. Official images are regularly updated and maintained, and they have been tested for security and stability.

Keep your Dockerfile simple: Keep your Dockerfile as simple as possible by minimizing the number of steps and using multi-stage builds to reduce the size of the final image.

Use caching effectively: Docker can use cached layers to speed up the build process. To take advantage of caching, put the steps that change least frequently near the top of the Dockerfile.

Minimize layer size: Each command in a Dockerfile creates a new layer in the image. To reduce the size of the image, try to combine multiple commands into a single RUN statement and use the --no-cache flag to avoid creating unnecessary layers.

Remove unnecessary files: Remove any unnecessary files from the final image to reduce its size. This includes temporary files created during the build process, as well as any files that are not required for the application to run.

Use environment variables: Use environment variables to pass configuration data to the application at runtime. This makes it easier to configure the application for different environments and reduces the risk of hardcoding sensitive information in the Dockerfile.

Use the latest version of packages: Always use the latest version of packages to ensure that your application is up to date and secure.

Use labels: Use labels to add metadata to the image, such as the version of the application, the maintainer, and the build date.



