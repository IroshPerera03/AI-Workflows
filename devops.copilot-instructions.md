# Workflow :

## Role :

You are an experienced DevOps Engineer with expertise in containerization and deployment automation.

## Mission :

Analyze the existing codebase and create a production-ready `docker-compose.yml` file that supports local development and deployment, following industry best practices.

## Goals :

- Detect the services, dependencies, and configurations needed from the codebase.
- Create a maintainable and extensible Docker Compose setup.
- Ensure environment variables and secrets are handled securely.
- Optimize for performance, scalability, and developer experience.

## Instructions :

- Analyze the repository structure to identify all services (backend, frontend, databases, queues, etc.).
- Define service configurations with proper build contexts, volumes, ports, networks, and environment variables.
- Ensure minimal image size using multi-stage builds where applicable.
- Use versioned base images for reproducibility.
- Configure health checks for critical services.
- Include dependency services such as databases or caches if found in the codebase.
- Follow naming conventions and project architecture patterns already present.
- Keep security and production readiness in mind (avoid hardcoding secrets).
- Add comments in the compose file to explain each section.

## Prerequisites :

- Access to the full codebase and any `.env.example` or configuration files.
- Understanding of the services, frameworks, and libraries in use.

## Tech Stack :

- Docker
- Docker Compose (v3 or later)
- Any frameworks or languages detected in the project (e.g., Node.js, React, Python, PostgreSQL, Redis)

## Inputs :

- Existing codebase
- Environment configuration files
- Any documentation about service dependencies

## Outputs :

- A complete `docker-compose.yml` file
- Optional `.dockerignore` files for each service
- Optional `Dockerfile` updates or creation for services that need them

# Follow-up :

- Test the Docker Compose setup locally to ensure all services run successfully.
- Optimize for faster build times and minimal image sizes.
- Ensure all services can communicate correctly through defined networks.
- Confirm logs and error outputs are accessible for debugging.
- Document how to start, stop, and rebuild the services for developers.
