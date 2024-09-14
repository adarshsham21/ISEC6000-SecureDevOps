Overview
This project walks through the process of setting up, deploying, and securing a microservices-based e-commerce application using Saleor. We leverage Google Cloud Platform (GCP), Docker, Kubernetes, and Trivy to handle deployment and security.

Tasks
Task 1: Setting Up Infrastructure
Google Cloud Kubernetes Cluster: We created a Kubernetes cluster on GCP using the Google Cloud Console to provision our resources.
kubectl: Installed and configured to manage the cluster from the command line.

GitHub Repository: The project code is hosted in a GitHub repository—ISEC6000-SecureDevOps.

Task 2: Deploying Microservices
Services Overview:

API: Runs the Saleor backend (image: ghcr.io/saleor/saleor:latest), exposed at port 9002.
Dashboard: Provides the admin interface (image: ghcr.io/saleor/saleor-dashboard:latest), available at port 9000.
PostgreSQL: Our database (image: postgres:12), exposed at port 5432.
Redis: Acts as a caching layer (image: redis:6), exposed at port 6379.
Worker: Handles background tasks using Celery (image: ghcr.io/saleor/saleor:latest).
Mailpit: Manages outgoing emails (image: axllent/mailpit), exposed at ports 1025 (SMTP) and 8025 (Web UI).
Jaeger: Provides distributed tracing (image: jaegertracing/all-in-one).

Deployment Steps:

Modify the docker-compose.yml file as needed.
Build and migrate the application using the following commands:

docker compose run --rm api python3 manage.py migrate
docker compose up
Access the services:
API: localhost:9002
Dashboard: localhost:9000
Storefront: localhost:3000

Admin Setup:

Created a superuser with the following credentials:
Email: admin@example.com
Password: admin

Task 3: Security

To ensure security, we scanned all Docker images with Trivy to identify any vulnerabilities. Here’s an example of the commands we used:


sudo trivy image ghcr.io/saleor/saleor:3.20
sudo trivy image ghcr.io/saleor/saleor-dashboard:latest
sudo trivy image redis:7.0-alpine
sudo trivy image jaegertracing/all-in-one:latest

Task 4: Architecture Overview
Services: The architecture includes the API, Dashboard, PostgreSQL, Redis, Worker, Mailpit, and Jaeger services.
Networks:
Backend Network: Connects the API, Redis, PostgreSQL, Worker, Jaeger, and Mailpit.
Frontend Network: Connects the Dashboard, isolating frontend from backend components.
Storage: Persistent volumes include saleor-db (PostgreSQL), saleor-media (API & Worker), and saleor-redis (Redis).

Conclusion
This project successfully demonstrates the deployment and security of a microservices-based e-commerce platform using Saleor. We used GCP, Kubernetes, Docker, and Trivy to handle infrastructure, application management, and security.
