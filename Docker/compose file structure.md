# Docker Compose File Structure

```yaml
# ===========================
# Docker Compose File Structure
# ===========================

services:                  # Top-level key - Defines all services (containers) in the application.

  web:                     # Service name (Service identifier) - The logical name of this container.

    image: nginx           # Service option - Specifies the Docker image used to create the container.

    build: .               # Service option - Builds an image from the Dockerfile in the current directory.

    container_name: web    # Service option - Assigns a custom name to the created container.

    ports:                 # Service option - Declares port mappings between the host and the container.

      - "80:80"            # List item (Port mapping) - Maps Host Port 80 to Container Port 80.

    volumes:               # Service option - Mounts files, directories, or Docker volumes into the container.

      - ./html:/usr/share/nginx/html
                            # List item (Volume mount)
                            # Host Path: ./html
                            # Container Path: /usr/share/nginx/html

    environment:           # Service option - Defines environment variables available inside the container.

      APP_ENV: production  # Environment variable (Key-value pair)
                            # Variable Name: APP_ENV
                            # Variable Value: production

    depends_on:            # Service option - Declares service dependencies (startup order).

      - db                 # List item (Dependency)
                            # The 'web' service depends on the 'db' service.

    networks:              # Service option - Connects this service to one or more Docker networks.

      - app-network        # List item (Network reference)
                            # Connects this container to the app-network.

    restart: always        # Service option (Restart policy)
                            # Automatically restarts the container if it stops.

# ---------------------------------------------------

  db:                      # Service name (Service identifier) - Database container.

    image: postgres:16     # Service option - Uses the PostgreSQL version 16 image.

    volumes:               # Service option - Mounts persistent storage.

      - db-data:/var/lib/postgresql/data
                            # List item (Named volume mount)
                            # Volume Name: db-data
                            # Mounted Inside Container: /var/lib/postgresql/data

# ===========================
# Volume Definitions
# ===========================

volumes:                   # Top-level key - Defines reusable Docker-managed volumes.

  db-data:                 # Named volume - Persistent storage managed by Docker.

# ===========================
# Network Definitions
# ===========================

networks:                  # Top-level key - Defines custom Docker networks.

  app-network:             # Network name - Creates a custom network named "app-network".

    driver: bridge         # Network option - Uses Docker's bridge network driver.

# ===========================
# Config Definitions
# ===========================

configs:                   # Top-level key (Optional)
                            # Stores configuration files that services can use.

# ===========================
# Secret Definitions
# ===========================

secrets:                   # Top-level key (Optional)
                            # Stores sensitive data such as passwords, API keys, or certificates.
```

---

# Line Types Summary

|YAML Line|Official Name|Description|
|---|---|---|
|`services:`|Top-level key|Groups all services (containers).|
|`web:`|Service name (Service identifier)|Logical name of a container.|
|`image:`|Service option|Specifies the Docker image.|
|`build:`|Service option|Builds an image from a Dockerfile.|
|`container_name:`|Service option|Sets a custom container name.|
|`ports:`|Service option|Declares port mappings.|
|`- "80:80"`|List item (Port mapping)|Maps the host port to the container port.|
|`volumes:`|Service option|Mounts storage into the container.|
|`- ./host:/container`|List item (Volume mount)|Mounts a host path or a named volume into the container.|
|`environment:`|Service option|Declares environment variables.|
|`APP_ENV: production`|Environment variable (Key-value pair)|Defines an environment variable inside the container.|
|`depends_on:`|Service option|Defines service startup dependencies.|
|`- db`|List item (Dependency)|References another service that should start first.|
|`networks:` (inside a service)|Service option|Connects a service to one or more networks.|
|`- app-network`|List item (Network reference)|Connects the service to a network.|
|`restart:`|Service option (Restart policy)|Controls when Docker automatically restarts the container.|
|`volumes:` (root level)|Top-level key|Defines Docker-managed named volumes.|
|`db-data:`|Named volume|Persistent storage managed by Docker.|
|`networks:` (root level)|Top-level key|Defines custom Docker networks.|
|`app-network:`|Network name|Name of the custom Docker network.|
|`driver:`|Network option|Specifies the network driver (e.g., `bridge`).|
|`configs:`|Top-level key|Defines configuration objects that services can use.|
|`secrets:`|Top-level key|Defines secret objects for sensitive data such as passwords and API keys.|