# Node.js Multi-Stage Docker Build

This Dockerfile demonstrates a multi-stage build for a Node.js application. The build process is divided into two stages:

1. **Build Stage**: Installs dependencies and compiles the application.
2. **Final Stage**: Copies the built application into a smaller, lightweight image.

## How to Build

```bash
docker build -t nodejs-app .
```

## Why Multi-Stage?

- **Smaller Images**: Only the necessary files are included in the final image.
- **Faster Builds**: Reusing layers and separating build and runtime stages reduce overall build time.

## .dockerignore

The `.dockerignore` file is configured to exclude unnecessary files from the build context, further optimizing the build process.


Explanation:

- First Stage (Build Stage): 
Uses a full Node.js environment to install dependencies and build the application.

    - COPY package*.json: Copies the package.json and package-lock.json files to install only necessary dependencies.
    - RUN npm install: Installs Node.js dependencies.
    - COPY .: Copies the rest of the source code.
    - RUN npm run build: Builds the application.

- Second Stage (Runtime Stage): 
Uses a lightweight Node.js image to run the application.
    - COPY --from=build: Copies the built application and node_modules from the build stage.
    - CMD ["node", "dist/index.js"]: Specifies the command to run the application.
