# CICD-Github
SYSTEM ARCHITECTURE
        Developer
            │
            │  Push Code
            ▼
      GitHub Repository
            │
            │  Trigger CI/CD
            ▼
      GitHub Actions Pipeline
            │
            │ Deploy Static Files
            ▼
        AWS S3 Bucket
            │
            ▼
        Live Website
 PROJECT STRUCTURE
 Static-Website-DevOps
│
├── index.html
├── styles.css
├── script.js
│
├── .github
│   └── workflows
│        └── deploy.yml
│
└── README.md
⚙️ CI/CD Pipeline Workflow

The project uses GitHub Actions to automate deployment.

Workflow Process

1️⃣ Developer pushes code to GitHub
2️⃣ GitHub Actions workflow is triggered
3️⃣ AWS credentials are fetched securely from GitHub Secrets
4️⃣ Files are uploaded to AWS S3
5️⃣ Website is updated automatically


⚡ GitHub Actions Workflow Example
name: Deploy Website to AWS S3

on:
  push:
    branches:
      - main

jobs:

  deploy:
    runs-on: ubuntu-latest

    steps:

    - name: Checkout Repository
      uses: actions/checkout@v3

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v2
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ap-south-1

    - name: Deploy to S3
      run: |
        aws s3 sync . s3://${{ secrets.S3_BUCKET_NAME }} --delete

     Step-by-Step Explanation (What You Say in Interview)
1. Trigger Stage

“The workflow is triggered whenever a developer pushes code to the main branch of the GitHub repository. This ensures that any new updates are automatically deployed without manual intervention.”

2. Runner Environment

“The pipeline runs on a GitHub-hosted runner using the ubuntu-latest environment. This runner provides a temporary Linux virtual machine where all pipeline tasks are executed.”

3. Code Checkout

“In the first step, the pipeline uses the GitHub Action actions/checkout@v3 to clone the repository into the runner environment. This allows the workflow to access all project files such as HTML, CSS, and JavaScript.”

4. AWS Authentication

“In the next step, AWS credentials are configured using the aws-actions/configure-aws-credentials action. The credentials are securely stored in GitHub Secrets to ensure they are not exposed in the code.”

Required secrets include:

AWS Access Key

AWS Secret Access Key

S3 Bucket Name

“This step allows the pipeline to authenticate with AWS services.”

5. Deployment to S3

“Finally, the pipeline uses the AWS CLI command aws s3 sync to upload the website files to the S3 bucket. The --delete flag ensures that any outdated files in the bucket are removed so that the deployed version always matches the latest code in the repository.”

End Result

“As a result, whenever new code is pushed to GitHub, the CI/CD pipeline automatically deploys the updated website to the S3 bucket, making the changes live without manual deployment.”   
