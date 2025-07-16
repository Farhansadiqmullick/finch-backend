# Django with PostgreSQL on Kubernetes

This setup includes the following components:

## 1. Django Application
- Runs in a container managed by Kubernetes
- Accessible via a web browser on a specific port
- Connects securely to the PostgreSQL database
- Configured using environment variables stored as secrets

## 2. PostgreSQL Database
- Runs in a container
- Only accessible within the cluster (not exposed to the public)
- Uses the same set of environment variables to stay in sync with the Django app

## 3. Secrets
- Stores sensitive data like database name, user, password, and Stripe API keys
- Helps keep configuration values secure and manageable

## 4. Services
- Provide networking inside the Kubernetes cluster
- One service exposes the Django app so it can be accessed from outside the cluster
- Another service keeps the PostgreSQL database reachable to Django but hidden from the outside world

## 5. Probes (Health Checks)
- Built-in health checks ensure the Django app is running properly
- Kubernetes uses these checks to restart the app automatically if something goes wrong

---

## 📦 Why This Matters

By containerizing Django and PostgreSQL and managing them with Kubernetes:
- We can deploy updates without downtime
- Our application becomes more portable and scalable
- Sensitive settings stay safe through Kubernetes secrets
- Logs, performance, and monitoring become easier to integrate

You can copy this content directly into a README.md file in your project. The Markdown formatting includes:

    Headers for each section

    Bullet points for lists

    A horizontal rule (---) for the section break

    An emoji in the "Why This Matters" section for visual interest