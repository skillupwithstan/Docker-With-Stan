This enhanced guide expands on the fundamental Docker concepts in your repository, introducing production-ready Docker Compose configurations, health checks, environment file management, and advanced networking.

**Core Docker Compose Commands**

```bash
# Start all services in the background (detached mode)
docker-compose up -d

# Stop all services and remove containers, default networks, and images
docker-compose down

# Stop services, remove containers, and ALSO remove named volumes (resets data)
docker-compose down -v

# Force a rebuild of the images before starting the containers
docker-compose up -d --build

# View and follow logs for all services in real-time
docker-compose logs -f

# View logs for a specific service (e.g., the mongodb service)
docker-compose logs -f mongodb

# Execute a command inside a running service's container (useful for debugging)
docker-compose exec my-app /bin/sh

# List all running containers managed by this compose file
docker-compose ps

# Restart a specific service without affecting others
docker-compose restart my-app

```

**Production-Ready Docker Compose Example**

This enhanced version of your MongoDB and Node.js application stack includes security best practices, dependency mapping, container health checks, and explicit network management.

```yaml
version: '3.8'

services:
  my-app:
    image: registryDomain/imageName:tag
    container_name: production_app
    build: 
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    env_file:
      - .env
    depends_on:
      mongodb:
        condition: service_healthy
    networks:
      - app-network
    restart: unless-stopped

  mongodb:
    image: mongo:latest
    container_name: mongodb_primary
    ports:
      - "27017:27017"
    env_file:
      - .env
    volumes:
      - db-data:/data/db
    networks:
      - app-network
    healthcheck:
      test: echo 'db.runCommand("ping").ok' | mongosh localhost:27017/test --quiet
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 15s
    restart: unless-stopped

  mongo-express:
    image: mongo-express:latest
    container_name: mongodb_express_ui
    ports:
      - "8081:8081"
    env_file:
      - .env
    depends_on:
      mongodb:
        condition: service_healthy
    networks:
      - app-network
    restart: unless-stopped

volumes:
  db-data:
    name: production_db_data

networks:
  app-network:
    driver: bridge

```

**Key Structural Enhancements Explained**

* **`version: '3.8'`**: Updating to a newer compose schema allows access to advanced features like complex health checks and extended resource limitations.
* **`env_file`**: Instead of hardcoding credentials directly in the YAML file (which is a security risk if committed to source control), this tells Docker to read variables from a `.env` file in the same directory.
* **`depends_on` (with `service_healthy`)**: Ensures that `my-app` and `mongo-express` will not even attempt to start until the `mongodb` container has fully initialized and passed its health check.
* **`healthcheck`**: Actively pings the database inside the MongoDB container to verify it is ready to accept connections, preventing your dependent apps from crashing on startup.
* **`restart: unless-stopped`**: Automatically restarts the container if it crashes or if the Docker daemon reboots, keeping your application highly available.
* **Explicit Networks**: Manually defining `app-network` with the `bridge` driver ensures strict isolation and allows you to easily connect other containers to this specific stack later.

**Secure Environment File (.env)**

Create this file in the same directory as your `docker-compose.yml`. **Never commit this file to GitHub**; add it to your `.gitignore`.

```bash
# .env file
# MongoDB Configuration
MONGO_INITDB_ROOT_USERNAME=admin
MONGO_INITDB_ROOT_PASSWORD=SuperSecurePassword123!

# Mongo Express Configuration
ME_CONFIG_MONGODB_ADMINUSERNAME=admin
ME_CONFIG_MONGODB_ADMINPASSWORD=SuperSecurePassword123!
ME_CONFIG_MONGODB_SERVER=mongodb

# Application Variables
PORT=3000
NODE_ENV=production

```
