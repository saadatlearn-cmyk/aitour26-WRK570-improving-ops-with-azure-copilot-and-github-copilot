# Use the official Python image as the base image
FROM python:3.12.7-alpine

# create a dedicated non-root user/group
RUN addgroup -S app && adduser -S app -G app

# Set the working directory to /app
WORKDIR /app

# Set the build argument for the app version number
ARG APP_VERSION=0.1.0

# Copy the requirements file into the container
COPY requirements.txt .

# Install the dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code into the container
COPY --chown=app:app . .

# Expose port 5001 for the FastAPI application
EXPOSE 5001

# Set the environment variable for the app version number
ENV APP_VERSION=$APP_VERSION

# descriptive metadata for security scans / image provenance
LABEL org.opencontainers.image.source="https://github.com/${GITHUB_REPOSITORY:-unknown}" \
      org.opencontainers.image.version="$APP_VERSION" \
      org.opencontainers.image.revision="${GIT_COMMIT_SHA:-unknown}"

# run as non-root
USER app

# Start the FastAPI application
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "5001"]