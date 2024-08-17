# First stage: Build the application
FROM python:3.9-slim AS build
WORKDIR /app
COPY requirements.txt ./
RUN pip install -r requirements.txt
COPY . .
RUN python setup.py install

# Final stage: Create the runtime image
FROM python:3.9-alpine
WORKDIR /app
COPY --from=build /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages
COPY --from=build /app /app

# Purging unnecessary files from the final image
RUN pip cache purge

CMD ["python", "app/main.py"]
