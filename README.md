# flask-ci-cd-assignment
============================= 


link: https://github.com/Sandhyashree1812/flask-ci-cd-assignment/blob/7d2d4fde97741ec679d5288d4150552fb3f02249/README.md


==========================  


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

<img width="412" height="146" alt="image" src="https://github.com/user-attachments/assets/1c665206-0f24-4115-85f0-3c828ae1f033" /> 

<img width="417" height="147" alt="image" src="https://github.com/user-attachments/assets/28216db1-9ed6-4113-b386-d3610a0f1908" />


  Then open your browser and go to: http://localhost:5000/

  <img width="811" height="517" alt="Screenshot 2026-08-16 204717" src="https://github.com/user-attachments/assets/81f8da52-289f-41de-af1d-86a9d013d44f" />

  Then open your browser and go to: http://localhost:5000/health

<img width="362" height="334" alt="image" src="https://github.com/user-attachments/assets/34b3bc2c-bdfa-4a3c-8fd1-a7a5074bbb91" />

================================== 

Step 5: In the terminal run : python -m pip install -r requirements.txt 

<img width="386" height="329" alt="image" src="https://github.com/user-attachments/assets/2aa74f54-15cc-4fbb-9055-232731ca0a01" />


<img width="395" height="66" alt="image" src="https://github.com/user-attachments/assets/b6c6235a-47c0-4212-bd8e-8075f883659e" />


Step 6: Create a Dockerfile with the below code and save it:

FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"] 

Step 6: Check Docker version: docker --version 

Open Docker Desktop

Step 1 — Build your Flask Docker image 

Run the command: docker build -t flask-cicd-app:test . 

<img width="392" height="340" alt="image" src="https://github.com/user-attachments/assets/1f069e4a-86e9-4024-ad0b-dad8e8d61031" />

<img width="407" height="113" alt="image" src="https://github.com/user-attachments/assets/8cf53256-48d7-4ee6-8f45-193f31326e4c" />

==================== 
Docker will perform below steps:

Dockerfile
    ↓
python:3.12-slim
    ↓
Install Flask + pytest
    ↓
Copy app.py
    ↓
Create image
==================== 

Step 2: Run: docker images

<img width="397" height="115" alt="image" src="https://github.com/user-attachments/assets/c811962f-9346-405c-9a1d-ddd63254f728" />

Step 3: Run the container: docker run -d --name flask-test -p 5000:5000 flask-cicd-app:test 

<img width="399" height="247" alt="image" src="https://github.com/user-attachments/assets/10f65889-0882-49c4-902c-5b031b034ea9" /> 

In this we can see:
flask-test and 0.0.0.0:5000->5000/tcp 

Step 4: Now ope in browser: http://localhost:5000/health 

means our Flask application is running in docker container. 

We finished the below:
app.py
   ↓
Dockerfile
   ↓
Docker build
   ↓
flask-cicd-app:test 



<img width="729" height="403" alt="image" src="https://github.com/user-attachments/assets/6c39a3d4-22d0-42c0-8c46-8d5d17fe9a64" />

Run :  docker ps -a

We will container id and the flask test :

cf3b8e33c6c0   d1c34368e804   "python app.py"          10 minutes ago   Up 10 minutes   0.0.0.0:5000->5000/tcp, [::]:5000->5000/tcp   flask-test

<img width="609" height="128" alt="image" src="https://github.com/user-attachments/assets/dc1ee737-51b0-4ba0-9f2c-ba7bc6d7e513" /> 
<img width="414" height="173" alt="image" src="https://github.com/user-attachments/assets/66f5572d-b13d-4b9a-81a3-969778541374" />


=================== 
Step 2 — Create Amazon ECR Repository
Go to the AWS Management Console.
Search for:
ECR
Select:
Elastic Container Registry
2. Open Repositories

In the left menu, click:

Repositories

Then click:

Create private repository, give name: flask-cicd-app.
Leave the other settings as default, and create.
Copy the Repository URI somewhere temporarily. We will need it for the Docker commands.

After creating repo we will do the following steps:

Local Docker Image
      │
      │ docker tag
      ▼
ECR-compatible image
      │
      │ aws ecr login
      ▼
AWS ECR
      │
      │ docker push
      ▼
flask-cicd-app
      │
      ▼
Image tagged with commit 

============================== 

We will check for AWS version and caller identity in Powershell:
 aws --version
aws sts get-caller-identity 


<img width="376" height="105" alt="image" src="https://github.com/user-attachments/assets/2794a44e-9fd7-4dfe-90e1-41b3c823405c" />

Copy the details of identity and save somewhere

============= 


Step 1 — Log in to ECR

Run this in PowerShell:
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 259072552251.dkr.ecr.us-east-1.amazonaws.com 

<img width="670" height="42" alt="image" src="https://github.com/user-attachments/assets/752bbf6b-66a9-467f-b6b1-d7aa4cfd7394" /> 

Step 2 — Tag your Docker image

Your existing local image is: flask-cicd-app:test 

docker tag flask-cicd-app:test 259072552251.dkr.ecr.us-east-1.amazonaws.com/flask-cicd-app:manual-test 
Check the image 

<img width="659" height="325" alt="image" src="https://github.com/user-attachments/assets/1500853b-8d4f-42ed-87c4-7cf5939dc04a" /> 

<img width="658" height="16" alt="image" src="https://github.com/user-attachments/assets/2a4d2e43-b486-4131-b33a-2e381bc76126" /> 

Step 3 — Push the image

docker push 259072552251.dkr.ecr.us-east-1.amazonaws.com/flask-cicd-app:manual-test

<img width="651" height="162" alt="image" src="https://github.com/user-attachments/assets/d1810cf0-b056-4073-b571-943b7a9a061d" />



Step 4 — Verify in AWS

Go to:

AWS Console → ECR → Repositories → flask-cicd-app

You should now see:
Image tag
manual-test

<img width="696" height="167" alt="image" src="https://github.com/user-attachments/assets/d82cba1d-ecea-4765-ad9e-2ef073e34f31" />

Once manual-test appears in ECR, we've completed the ECR portion.

================= 

Step 4 — Prepare EC2

The assignment requires:

EC2 instance with Docker installed and running, and an IAM role allowing it to pull from ECR.

We'll do this carefully.

1. Go to EC2

In AWS Console:

EC2 → Instances → Launch instances

Create a new instance.

Name: Flask-CICD-EC2

SSH          22     My IP
Custom TCP   5000   My IP
Add port 5000

Add security group rule
Type: Custom TCP
Port: 5000
Source: My IP

So you'll have:

Type	Port	Source
SSH	22	My IP
Custom TCP	5000	My IP

Port 5000 is where our Flask application will run. 

Launch instance


<img width="616" height="398" alt="image" src="https://github.com/user-attachments/assets/23498345-3f70-4c09-aafc-eae9a3ed4438" /> 


<img width="936" height="204" alt="image" src="https://github.com/user-attachments/assets/ac2875ed-6241-4d8c-92dd-63b9c0613ff9" /> 
===================== 

Launch the instance: check for status as running. and both the checks passed

<img width="712" height="332" alt="image" src="https://github.com/user-attachments/assets/699453a6-58c5-4273-b929-ce4d895412c8" /> 

<img width="730" height="250" alt="image" src="https://github.com/user-attachments/assets/bbebe1ce-4ef2-4106-bc14-43bb74dd544a" /> 

====================== 

EC2 instance needs permission to pull the Docker image you already uploaded to ECR.
We will attach: AmazonEC2ContainerRegistryReadOnly to an IAM role.

Step 1 — Go to IAM

In AWS Console:
IAM → Roles → Create role
Trusted entity type, choose: AWS service
Service or use case, select: EC2
Then click: Next

<img width="920" height="362" alt="image" src="https://github.com/user-attachments/assets/76b18241-a205-4b56-8462-a9074dbdc78a" />

<img width="918" height="251" alt="image" src="https://github.com/user-attachments/assets/fd004581-c82d-49a3-804b-165e80a02646" /> 

Step 3 — Add permission

Search for: AmazonEC2ContainerRegistryReadOnly
click Next.

Name he role: EC2ECRPullRole
Description: Allows EC2 to pull Docker images from Amazon ECR.
Click create role/

<img width="706" height="80" alt="image" src="https://github.com/user-attachments/assets/a97cf28c-234d-4895-a424-e07fa13a5ad1" />

 ===================== 

 Step 5 — Attach the role to your EC2

Go back to:
EC2 → Instances
Select: Flask-CICD-EC2 
Click: Actions → Security → Modify IAM role
You should see the role dropdown, select EC2ECRPullRole

<img width="794" height="256" alt="image" src="https://github.com/user-attachments/assets/5ea379cc-b7bf-44b0-9970-61f374290a37" /> 

Click: Actions → Security → Modify IAM role
You should see the role dropdown, select EC2ECRPullRole 

Click update the IAM role 

<img width="944" height="328" alt="image" src="https://github.com/user-attachments/assets/7fcc65e2-c234-42c6-9397-75e64ca029fb" /> 

<img width="737" height="176" alt="image" src="https://github.com/user-attachments/assets/451ed428-962a-4ce9-b8e8-986d1673e2e6" />


The resulting architecture is:

<img width="422" height="196" alt="image" src="https://github.com/user-attachments/assets/f54dfb28-12af-4d1a-8bb7-2f35f0e6053c" /> 

===================== 

EC2 Instance Connect
Go to AWS Console → EC2 → Instances.
Select Flask-CICD-EC2.
Click Connect.
Select EC2 Instance Connect.
Username should be: ec2-user

Click Connect.
If the browser terminal opens, run: docker --version

If EC2 Instance Connect does not work:
Temporarily allow SSH from anywhere

Go to: EC2 → Instances → Flask-CICD-EC2

Then: Security → Security groups → click your security group

Go to: Inbound rules → Edit inbound rules

Change the Source to: Anywhere-IPv4

Then click:

Save rules 

Try EC2 Instance Connect again

Go back to:

EC2 → Instances → Flask-CICD-EC2 → Connect

Select:

EC2 Instance Connect

Username: ec2-user
Click: Connect

You should get a terminal ending with something like: [ec2-user@ip-172-31-80-208 ~]$ 

1. let's prepare this EC2 instance to pull your Docker image from ECR.

Install Docker on Amazon Linux 2023: sudo dnf install -y docker

Start Docker: sudo systemctl start docker


<img width="926" height="303" alt="image" src="https://github.com/user-attachments/assets/bce5825b-1c29-45ae-b3bd-46c6d64c06d6" /> 

Enable it at boot: sudo systemctl enable docker
Check: sudo systemctl status docker
allow ec2-user to use Docker without sudo: sudo usermod -aG docker ec2-user 

exit the EC2 terminal and reconnect using EC2 Instance Connect so the group membership takes effect.

1. docker --version
2. Check AWS CLI
   Once Docker is ready, run: aws --version


   <img width="922" height="350" alt="image" src="https://github.com/user-attachments/assets/3feb6d9a-fff8-4cb4-99b2-41ba70e12f8c" />

 3. Login to ECR
  aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 259072552251.dkr.ecr.us-east-1.amazonaws.com




4. Pull your ECR image:
   We already pushed: flask-cicd-app:manual-test

   Run: docker pull 259072552251.dkr.ecr.us-east-1.amazonaws.com/flask-cicd-app:manual-test

   This canbe seen:
   <img width="861" height="27" alt="image" src="https://github.com/user-attachments/assets/1d5fa400-557b-41f5-952a-08920fd1b6d0" />



<img width="929" height="276" alt="image" src="https://github.com/user-attachments/assets/ecea0134-522a-44fd-9cf3-f3fe04ab15ec" /> 

5. Run the ECR image on EC2

   docker run -d --name flask-app -p 5000:5000 259072552251.dkr.ecr.us-east-1.amazonaws.com/flask-cicd-app:manual-test
   docker ps
<img width="929" height="158" alt="image" src="https://github.com/user-attachments/assets/dffc10b8-75e3-4c97-92f8-15c91241f641" />

6. Verify /health on EC2:
   curl http://localhost:5000/health

   <img width="553" height="50" alt="image" src="https://github.com/user-attachments/assets/067e06c4-26da-420f-993a-8bc3685956ef" />

   <img width="936" height="311" alt="image" src="https://github.com/user-attachments/assets/f7146295-4a7b-4001-8b90-a6a3a56f5feb" />



Checking in browser:
application running on EC2, not just locally.

<img width="486" height="325" alt="image" src="https://github.com/user-attachments/assets/1f2ddc39-2eb1-497a-b108-ac13f07974e4" /> 

===================================== 

Building the actual CI/CD Pipeline:

GitHub push to main
        ↓
Jenkins
        ↓
1. Checkout
        ↓
2. Install dependencies
        ↓
3. pytest
        ↓
4. Docker build
   tag = Git commit SHA
        ↓
5. Push to ECR
        ↓
6. SSH to EC2
        ↓
7. Pull new image
        ↓
8. Stop/remove old container
        ↓
9. Run new container
        ↓
10. /health verification
        ↓
11. Email SUCCESS/FAILURE

    =====================================

1. Create the .ssh folder

   In PowerShell run: New-Item -ItemType Directory -Force -Path "C:\Users\sandy\.ssh"

   2. Generate the Jenkins key 
      Run: ssh-keygen -t ed25519 -C "jenkins-ec2-deploy" -f "C:\Users\sandy\.ssh\jenkins-ec2"
   3. Enter passphrase (empty for no passphrase): press enter
   4. Enter same passphrase again: press enter
   5. Verify both files

         Get-ChildItem "C:\Users\sandy\.ssh\jenkins-ec2*"


    <img width="659" height="364" alt="image" src="https://github.com/user-attachments/assets/4ffc8864-2114-4c62-8165-85900f513399" />

   <img width="612" height="136" alt="image" src="https://github.com/user-attachments/assets/6d502343-1ad3-4031-bdbd-36c3bebe30d8" />
   



6. Display ONLY the public key

 run: Get-Content "C:\Users\sandy\.ssh\jenkins-ec2.pub"
 ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHp2sQYhddmXx7l2FvRiwpm8K1QAUybtI7sEkMfMJpvI jenkins-ec2-deploy

 copy the entire line. 


7. <img width="669" height="42" alt="image" src="https://github.com/user-attachments/assets/9f89910f-3d03-4dbf-8480-e81569294f80" />

8.  7. Go back to your EC2 terminal

      Open: AWS Console → EC2 → Instances → Flask-CICD-EC2 → Connect → EC2 Instance Connect

      run: mkdir -p ~/.ssh
           chmod 700 ~/.ssh
           nano ~/.ssh/authorized_keys
           add Add your public key which was copied:
     ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHp2sQYhddmXx7l2FvRiwpm8K1QAUybtI7sEkMfMJpvI jenkins-ec2-deploy
    


    <img width="918" height="368" alt="image" src="https://github.com/user-attachments/assets/6b69c2fe-200e-4569-9957-363232ac86e7" />

    Press cntrl+ O and then cntrl + X

    run: chmod 600 ~/.ssh/authorized_keys


Check the current EC2 Public IPv4
Go to: EC2 → Instances → Flask-CICD-EC2 → Details

Find: Public IPv4 address
      Copy the value shown there.
Test port 22: In PowerShell, replace YOUR_CURRENT_IP with the value from EC2:
    
 Test-NetConnection 100.53.92.173 -Port 22 

 <img width="446" height="155" alt="image" src="https://github.com/user-attachments/assets/97b638f4-4df7-4221-9513-2b96215e4a83" /> 


Connect using EC2 Instance Connect again: 

Go to: EC2 → Instances → Flask-CICD-EC2 → Connect → EC2 Instance Connect
You successfully used this earlier, so it should give you:
[ec2-user@ip-172-31-80-208 ~]$

Check the Jenkins public key
Inside the EC2 terminal, run:
cat ~/.ssh/authorized_keys
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys 

Fix permissions:
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chown -R ec2-user:ec2-user ~/.ssh

Check EC2 user's shell:
getent passwd ec2-user

<img width="936" height="213" alt="image" src="https://github.com/user-attachments/assets/04479328-abed-44b8-bbf6-366667047e04" /> 

<img width="936" height="247" alt="image" src="https://github.com/user-attachments/assets/ca20d4ae-fff4-4fef-941f-0ae23580e5b8" /> 

<img width="488" height="69" alt="image" src="https://github.com/user-attachments/assets/7927810f-2e4c-4ced-a734-b2337a940bcb" /> 

Type exit, logout.
we will get : PS C:\Users\sandy>
git --version
docker --version 
aws --version

<img width="716" height="190" alt="image" src="https://github.com/user-attachments/assets/de749899-76c5-499e-b77c-d527a5ef4c6a" /> 


docker network create jenkins
Open Windows PowerShell:
Open a new PowerShell window and run: docker info 
 run: docker network create jenkins
 run: docker volume create jenkins_home
 run: docker pull jenkins/jenkins:lts-jdk17


<img width="503" height="371" alt="image" src="https://github.com/user-attachments/assets/e1115a1d-9ad9-4885-96da-3a05a3d38324" />

<img width="644" height="323" alt="image" src="https://github.com/user-attachments/assets/b74ed44d-2114-4f56-9870-51b3edf694bb" />






    

