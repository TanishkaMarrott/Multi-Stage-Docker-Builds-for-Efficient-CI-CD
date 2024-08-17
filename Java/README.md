# Java Multi-Stage Docker Build

This directory contains an example of a multi-stage Docker build for a Java application using Maven. The Dockerfile is designed to optimize the build and runtime phases by separating them into distinct stages, resulting in a smaller and more efficient Docker image.

## Dockerfile Overview

The Dockerfile is divided into two stages:

1. **Build Stage:**
   - **Base Image:** `maven:3.8.1-openjdk-11`
   - **Purpose:** This stage handles compiling the Java application and packaging it into a JAR file. By using Maven, dependencies are downloaded, and the application is built without including unnecessary files in the final image.
   - **Key Steps:**
     - Copies the `pom.xml` file and downloads dependencies.
     - Copies the source code and compiles it into a JAR file.

2. **Runtime Stage:**
   - **Base Image:** `openjdk:11-jre-slim`
   - **Purpose:** This stage creates the final Docker image, containing only the JRE and the packaged JAR file, ensuring the image is as lightweight as possible.
   - **Key Steps:**
     - Copies the JAR file from the build stage.
     - Sets the command to run the JAR file.

## How to Build and Run

### Step 1: Build the Docker Image
To build the Docker image, navigate to this directory and run the following command:

```bash
docker build -t java-app .
```

### Step 2: Run the Docker Container
Once the image is built, you can run the Docker container using:

```bash
docker run -p 8080:8080 java-app
```

This command will start the application, exposing it on port 8080.

## Advantages of Multi-Stage Builds

- **Optimized Image Size:** By separating the build and runtime environments, the final Docker image is much smaller.
- **Security:** Only the necessary runtime dependencies are included, reducing the attack surface.
- **Efficiency:** By caching dependencies and separating stages, build times are improved, especially for subsequent builds.

## .dockerignore Configuration

The `.dockerignore` file is configured to exclude unnecessary files and directories from the Docker build context, further optimizing the build process:

- **`target/`**: Excludes compiled classes and JAR files that aren't needed in the build context.
- **`Dockerfile` & `.dockerignore`**: Prevents these files from being copied into the final image.

## Conclusion

This example demonstrates how to use multi-stage Docker builds to create efficient, secure, and optimized Docker images for Java applications. Feel free to modify and adapt the Dockerfile to suit your project's specific needs.

---

For any questions or contributions, please feel free to open an issue or submit a pull request.