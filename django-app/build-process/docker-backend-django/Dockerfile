FROM python:3.12-slim

# Set environment variables
ENV PYTHONUNBUFFERED=1
ENV DJANGO_SETTINGS_MODULE=backend.settings

# Install system dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        git \
        build-essential \
        binutils \
        libproj-dev \
        gdal-bin \
        supervisor && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set work directory
WORKDIR /app

# Copy application files
COPY django-app /app

# Copy boot script and make it executable
COPY django-app/build-process/docker-backend-django/scripts/boot.sh /app/scripts/boot.sh
RUN chmod +x /app/scripts/boot.sh

# Copy supervisor configuration
COPY django-app/build-process/docker-backend-django/config/supervisor-backend.conf /etc/supervisor/conf.d/supervisor-backend.conf

# Install Python dependencies
RUN pip install --upgrade pip && \
    pip install -r /app/requirements/requirements.txt

# Expose the development port
EXPOSE 8000

# Run the application
ENTRYPOINT ["/app/scripts/boot.sh"]


