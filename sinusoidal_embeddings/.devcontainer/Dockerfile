FROM python:3.11.9-slim
# Set environment variable to avoid tzdata prompts
ENV DEBIAN_FRONTEND=noninteractive

# Prevents Python from writing pyc files.
ENV PYTHONDONTWRITEBYTECODE=1

# Keeps Python from buffering stdout and stderr to avoid situations where
# the application crashes without emitting any logs due to buffering.
ENV PYTHONUNBUFFERED=1

# Set the working directory in the container
WORKDIR /app

# Create a non-privileged user that the app will run under.
ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --uid "${UID}" \
    appuser

# Switch to root user to install system dependencies
USER root

# Install GCC (if needed for building other dependencies)
RUN apt-get update && apt-get install -y gcc && \
    apt-get install -y libgl1-mesa-glx && \
    apt-get install -y libglib2.0-0 libsm6 libxrender1 libxext6 && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install Poetry and configure it
RUN pip install poetry && \
    poetry config virtualenvs.create false

# Switch back to the non-privileged user to run the application.
USER appuser