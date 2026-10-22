# Exercise: Build Your Own CI/CD Pipeline

Fuente: <https://learn.udacity.com/nd907?version=1.0.2&partKey=cd15155&lessonKey=28f80990-6f3d-4192-96bf-ddf26baa0cf5&conceptKey=58b5420c-fbdd-44e7-878b-f27a1821d632>

Contenido visible copiado literalmente de Udacity el 22 de octubre de 2026. No se ha completado ni resuelto la actividad.

Your team has developed a machine learning API for classifying bean diseases. Currently, every time a developer makes a change, someone must manually run tests, build a new Docker container, push it to the container registry, and deploy it to the cloud. This process is slow, inconsistent, and prone to human error. To improve this workflow, you will build an automated CI/CD pipeline that handles these steps reliably.

The goal of this exercise is to build a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline using GitHub Actions. This pipeline will automatically run code quality checks, build a Docker image, and deploy your application whenever you push new code to the `main` branch. This hands-on experience will demonstrate how to automate the ML Ops lifecycle from code commit to production deployment.

## Requirements

Your task is to create and configure a GitHub Actions workflow that automates the deployment of the beans API.

- **Repository setup**: Create a new public GitHub repository and populate it with the provided starter files. Clone this new repository into your workspace to begin working.
- **Configure secrets**: The pipeline needs credentials to interact with AWS services. You must configure the following as secrets in your GitHub repository settings under **Actions**:
  - `AWS_ACCESS_KEY_ID`
  - `AWS_SECRET_ACCESS_KEY`
  - `AWS_SESSION_TOKEN`
  - `AWS_REGION`
  - `ECR_REGISTRY`
- **Complete the workflow**: Fill in the missing sections of the `ci.yml` file and place it in the correct directory (`.github/workflows/`) to be recognized by GitHub Actions. The workflow's `env` block defines the resource names used throughout: `ECR_REPOSITORY`, `TASK_DEFINITION`, and `CONTAINER_NAME` are all set to `beans-api`, `ECS_SERVICE` is `beans-api-service`, and `ECS_CLUSTER` is `beans-api-cluster`. The workflow must contain four sequential jobs:
  1. **lint**: This job should install dependencies and run the `ruff` linter against the Python source files to check for style and quality issues.
  2. **quality-check**: This job runs the `quality_check.py` script. The pipeline must fail if this script exits with a non-zero status code.
  3. **build**: This job builds the Docker image for the `beans-api` service, and must only run after both `lint` and `quality-check` succeed. It must tag the image with the first eight characters of the commit SHA and a timestamp, push it to the `beans-api` Amazon ECR repository, and then tag and push the same image as `:latest`.
  4. **deploy**: This job depends on the successful completion of the build job. It should download the current `beans-api` Amazon ECS task definition, update the `beans-api` container definition to use the new `:latest` Docker image, and deploy the new task definition to the `beans-api-service` service running on the `beans-api-cluster` ECS cluster.
- **Verify the pipeline**: After completing the workflow file, commit and push your changes to the `main` branch. Navigate to the **Actions** tab in your GitHub repository to monitor the pipeline's execution and confirm that it completes successfully.

## Starter Files

You are provided with the following files to complete the exercise:

- `main.py`: The FastAPI application code for the beans classifier.
- `quality_check.py`: A Deepchecks script for data and model validation.
- `ci.yml`: A template for your GitHub Actions workflow with `TODO` markers indicating where to add your implementation.
- `requirements.txt`: Python dependencies for the FastAPI application.
- `requirements-quality.txt`: Python dependencies for the quality check script.
- `Dockerfile`: The file that defines how to build the application's container image.
