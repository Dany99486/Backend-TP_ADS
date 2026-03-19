# Movie Recommendation System

This project consists of a movie recommendation system developed for the Automation and Software Development course (MEI 2025/2026) at the University of Coimbra.

## CI/CD Pipeline

The DevOps infrastructure is based on GitHub Actions, ensuring that all changes to the `main`, `staging`, and `development` branches are automatically validated and integrated.

### Workflow Structure

The pipeline is divided into two main jobs: **build-and-test** and **deploy**.

#### Job: Build and Test
This job performs rigorous code validation in a Python 3.12 environment:

* **Static Analysis and Linting:**
    * **Ruff Linter:** Used to ensure compliance with style standards and detect syntax errors.
    * **Xenon:** Analyzes code cyclomatic complexity, failing the build if modules exceed defined limits (`-b D -m C -a C`).
    * **Bandit:** Performs static security analysis to identify vulnerabilities such as SQL injections or exposed credentials.
    * **Pip-audit:** Checks the `requirements.txt` file against known vulnerability databases (CVEs).

* **Integration Tests:**
    * **Docker Compose:** Starts the database and API services in an isolated environment.
    * **Netcat (nc):** Implements a synchronization mechanism that blocks test execution until the API port is active.
    * **Pytest:** Runs integration tests against the functional system.

* **Artifact Management:**
    * After validation, a Docker image of the backend is built and pushed to the GitHub Container Registry (GHCR).

#### Job: Deploy
Conditionally executed after the success of the previous job:
* **Webhook Trigger:** Sends a signal to the Render platform.
* **Automatic Update:** Render downloads the new image from GHCR and restarts the service without manual intervention.

---

## System Architecture

The system uses a container-based architecture, detailed in the project's C4 diagram:
* **Frontend:** Single Page Application (SPA) developed with React, Vite, and TailwindCSS.
* **Backend:** RESTful API built with Python Flask.
* **Database:** PostgreSQL with SQLAlchemy ORM.
* **Hosting:** Render Cloud Platform.

## Main Features

* User registration and authentication with security validation.
* Catalog management with search by title, author, and genre filtering.
* Rating system (1-5 stars) with real-time average calculation.
* Personalized recommendation engine based on user interests.
* AI-generated features: Watchlist, favorites, text reviews, and viewing history.

---

## Team

* **DevOps:** Daniel Bravo
* **Backend:** Tiago Anjos, José Rodrigues
* **Frontend:** Diogo Fatia, Sofia Yankova
