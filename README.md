# Java Application Dockerfile
This is an example multi-stage Dockerfile for building and running a Java application. The Dockerfile uses two stages: the first stage builds the application using Maven, and the second stage creates a smaller runtime image with only the JRE and the built application JAR file.

## Prerequisites
Docker installed on your local machine.
## Usage
1. Clone this repository to your local machine:
```bash
git clone https://github.com/AkshayGarad/java-dockerfile.git
```
2. Navigate to the project directory:
```bash
cd java-dockerfile
```
3. Build the Docker image using the following command:
```bash
docker build -t myapp:latest .
```
This will build the Docker image with the tag `myapp:latest`.
4. Run the Docker container using the following command:
```bash
docker run -p 8080:8080 myapp:latest
```
This will start the Docker container and map port 8080 on the container to port 8080 on your local machine.
5. Access the application by navigating to `http://localhost:8080` in your web browser.

## Dockerfile
This repository includes a Dockerfile that uses multi-stage builds to reduce the size of the final Docker image. The Dockerfile has two stages:

- Stage 1: Build the application using Maven.
- Stage 2: Create a smaller runtime image with only the JRE and the built application JAR file.

To use this Dockerfile for your own Java application, you can modify the following lines to reference your application's name and JAR file:
```bash
COPY --from=build /app/target/myapp.jar ./myapp.jar
CMD ["java", "-jar", "myapp.jar"]
```
Replace `myapp` with the name of your application, and replace `myapp.jar` with the name of your application's JAR file.

## Let's break down what's happening in each stage:

## Stage 1: Build the application

- We start with a Maven image that includes JDK 11 and set it as our build stage.
- We create a working directory `/app` and copy the `pom.xml` file into it.
- We download all dependencies required to build the application using `mvn dependency:go-offline`.
- We copy the source code from `src/` into the working directory.
- We build the application using `mvn package`, which creates a JAR file in the `target/` directory.

## Stage 2: Create the runtime image

- We start with a Java Runtime Environment (JRE) image that includes JDK 11.
- We create a working directory `/app`.
- We copy the `myapp.jar` file from the build stage to the working directory in the runtime stage using the `--from=build` option to reference the build stage.
- We set the default command to run the application using the `CMD` instruction.

This multi-stage Dockerfile allows us to separate the build process and the runtime environment into two separate stages, which can reduce the size of the final image by only including the files needed to run the application. In this example, we use Maven to build the application in the first stage and then use only the JRE in the second stage to create a smaller runtime image.