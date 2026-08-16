# flask-ci-cd-assignment

Graded  Assignment on CI/CD Pipeline 
Objective 
Build a CI/CD pipeline using either Jenkins or GitHub Actions (your choice, not both)  that 
automatically tests a Python Flask application, packages it into a Docker image, pushes that 
image to Amazon ECR, deploys it by running the container on an EC2 instance, and sends an 
email notification with a customized message reporting whether the run succeeded or failed. 
This assignment tests whether you can wire together source control, automated testing, 
containerization, a container registry, a real compute target, and operational feedback 
(notifications) into one working pipeline the core CI/CD skill set expected in a DevOps role. 

Architecture overview 
Developer push 
      │ 
      ▼ 
 ┌───────────────┐     ┌──────────┐     ┌─────────────┐     ┌──────────────┐ 
 │Jenkins / GH   │ →   │ Test     │ →   │ Build image │ →   │ Push to ECR  │ 
 │Actions trigger│     │ (pytest) │     │ (Docker)    │     │              │ 
 └───────────────┘     └──────────┘     └─────────────┘     └──────┬───────┘ 
                                                                   │ 
                                                                   ▼ 
                                                       ┌───────────────────────┐ 
                                                       │ Deploy to EC2:        │ 
                                                       │  - connect to instance│ 
                                                       │  - docker pull (ECR)  │ 
                                                       │  - docker stop/rm old │ 
                                                       │  - docker run new     │ 
                                                       └───────────┬───────────┘ 
                                                                   │ 
                                                                   ▼ 
                                                       ┌───────────────────────┐ 
                                                       │ Email notification:   │ 
                                                       │  success or failure,  │ 
                                                       │  with build details   │ 
                                                       └───────────────────────┘ 
© 2025 HeroX Private Limited. All rights reserved                                                   1 
Requirements 
1. Application (Github Repo LINK) 
● Use a simple Python Flask web application with at least one health/status endpoint (e.g. 
/health) that the deployment step can use to confirm the container actually started 
correctly on EC2. 
● Include a requirements.txt and a pytest test suite covering at least the core routes 
(success and failure cases). 
● Include a Dockerfile that builds a runnable image of the application. 
2. AWS prerequisites (set up manually before the pipeline runs) 
● An ECR repository to hold the built images. 
● An EC2 instance (Amazon Linux 2023 or Ubuntu) with: 
○ Docker installed and running 
○ An IAM instance role attached with permission to pull from ECR 
(AmazonEC2ContainerRegistryReadOnly or a scoped equivalent) 
○ A security group allowing inbound traffic on the application's port (and SSH/port 
22 only if you're using the SSH-based deploy method below) 
● A way for the pipeline to reach the EC2 instance to trigger the deployment — choose one: 
○ SSH-based: pipeline SSHes into the instance using a stored private key and runs 
the docker pull/run commands directly, or 
3. Pipeline stages 
Your pipeline (Jenkins or GitHub Actions) must implement, in order: 
1. Checkout — pull the latest source code 
2. Install dependencies — pip install from requirements.txt 
3. Test — run the pytest suite; the pipeline must stop here (not proceed to build/deploy) if 
any test fails 
4. Build — build the Docker image, tagged with the Git commit SHA (not just latest, so every 
deployed image is traceable to a commit) 
5. Push to ECR — authenticate to ECR and push the tagged image 
6. Deploy to EC2 — connect to the EC2 instance (SSH or SSM) and: 
○ pull the new image from ECR 
○ stop and remove the currently running container (if any) 
○ run the new container, mapping the application port 
○ verify the app actually came up (e.g. curl the /health endpoint from the pipeline or 
from the instance itself) — this is your deploy-verification gate; a container that 
starts but crashes immediately should still be reported as a failed deployment 
© 2025 HeroX Private Limited. All rights reserved                                                   
2 
7. Notify — send an email reporting the outcome (see Section 5) 
4. Triggers 
● The pipeline must trigger automatically on every push to the main branch of the 
repository (GitHub webhook for Jenkins; on: push for GitHub Actions). 
5. Email notifications with customized messages 
This is graded separately from "a notification exists" — the email content must differ 
meaningfully by outcome and include real build details, not a generic template. At minimum: 
On success, the email must include: 
● A clear success indicator (e.g. subject line prefixed ) 
● The Git commit SHA and branch that was deployed 
● The Docker image tag that was pushed to ECR 
● The EC2 instance/target that was updated 
● A link back to the pipeline run 
On failure, the email must include: 
● A clear failure indicator (e.g. subject line prefixed ) 
● Which stage failed (test / build / push / deploy) — not just "the pipeline failed" 
● The Git commit SHA and branch 
● A link to the pipeline run/logs so the failure can be investigated immediately without 
digging through the CI tool's UI first 
Use environment/credential-stored SMTP settings (or your CI tool's native email step) — never 
hardcode email credentials in the pipeline file. 
6. Secrets management 
Store all sensitive values (AWS credentials or role ARN, SSH private key or EC2 host details) in 
your CI tool's credential store (Jenkins Credentials, or GitHub Secrets) — never commit them to 
the repository. 
7. Documentation 
Update the repository's README.md to cover: 
● Prerequisites (AWS resources, IAM permissions, EC2 setup) 
● How to configure the pipeline's required secrets 
● How the deploy step connects to EC2 (SSH or SSM) and why you chose that method 
3 
© 2025 HeroX Private Limited. All rights reserved                                                   
● How to reproduce a deployment manually if the pipeline were unavailable 
Deliverables 
1. GitHub repository containing: 
○ The Flask application, Dockerfile, and pytest test suite 
○ The pipeline definition (Jenkinsfile or .github/workflows/*.yml) 
○ Updated README.md 
2. Screenshots or a short screen recording showing: 
○ A full pipeline run, all stages green, ending in a successful EC2 deployment 
○ The success email received 
○ At least one intentionally broken run (e.g. a failing test) showing the pipeline 
stopping early and the failure email received, with the correct failed-stage 
information in it 
3. A text/Word/PDF file containing the repository link, submitted via Vlearn. 

Submission instructions 
● Ensure your assignment is fully completed and pushed to GitHub. 
● Share the repository link in a text, Word, or PDF file. 
● Submit the file via Vlearn. 

========================================= 

Assignment:  CI/CD Pipeline 
===========================


Final Architecture:





<img width="839" height="661" alt="Screenshot 2026-08-16 211110" src="https://github.com/user-attachments/assets/cb582633-4617-441d-b1f0-3310e58eaf5d" />


<img width="320" height="182" alt="image" src="https://github.com/user-attachments/assets/a8ffa52d-1efd-436b-8026-68a8bd1bebe7" />


========================== 

Create a folder in VS code: flask-cicd assignment
create the below files in this folder:

flask-cicd-pipeline/
│
├── app.py
├── requirements.txt
├── Dockerfile
├── test_app.py
├── Jenkinsfile
└── README.md

Steps:


Step 1: Create the Flask application: app.py with the below code:

from flask import Flask, jsonify

app = Flask(__name__)


@app.route("/")
def home():
    return "Flask CI/CD Application is Running"


@app.route("/health")
def health():
    return jsonify({
        "status": "healthy"
    }), 200


@app.route("/error")
def error():
    return jsonify({
        "status": "error"
    }), 500


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)

============================= 

Step 2: create requirements.txt with the below code: 
Flask==3.1.2
pytest==8.4.1

================== 
Step 3: Create test file: test_app.py with the below code:

import pytest
from app import app


@pytest.fixture
def client():
    app.config["TESTING"] = True

  with app.test_client() as client:
        yield client


def test_home(client):
    response = client.get("/")
    assert response.status_code == 200
    assert b"Flask CI/CD Application is Running" in response.data


def test_health(client):
    response = client.get("/health")
    assert response.status_code == 200
    assert response.json["status"] == "healthy"


def test_error(client):
    response = client.get("/error")
    assert response.status_code == 500
    assert response.json["status"] == "error"


def test_invalid_route(client):
    response = client.get("/does-not-exist")
    assert response.status_code == 404

============================== 

Step 4: Open the VS Code terminal in this folder and run:

  python --version
  python -m pip install Flask
  python app.py

  Then open your browser and go to: http://localhost:5000/

  <img width="811" height="517" alt="Screenshot 2026-08-16 204717" src="https://github.com/user-attachments/assets/81f8da52-289f-41de-af1d-86a9d013d44f" />

  Then open your browser and go to: http://localhost:5000/health

<img width="362" height="334" alt="image" src="https://github.com/user-attachments/assets/34b3bc2c-bdfa-4a3c-8fd1-a7a5074bbb91" />
