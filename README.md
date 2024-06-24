# Web-Application-Deployment-
## Task: Deploy a scalable web application on AWS cloud

## Objective:
Deploy a scalable web application on AWS Cloud using various AWS services, ensuring high availability, performance, and security. You can use AWS S3 for hosting the static site, AWS CloudFront for content delivery, and GitHub Actions for automating the deployment process. Here's a step-by-step guide:

## Prerequisites
1.**AWS Account:** Sign up if you don’t have one.

2.**AWS CLI:** Install and configure it on your local machine.

3.**GitHub Account:** Create a repository for your static site.

### Step 1: Create an S3 Bucket

1.**Log in to the AWS Management Console.**

2.**Navigate to S3** and click **Create bucket.**

3.**Bucket Settings:**

- **Bucket name:** Enter a unique name (e.g., my-static-site-bucket).
        
- **Region:** Choose your preferred region.
    
4.**Block Public Access Settings:** Uncheck "Block all public access" and acknowledge the warning.

5.**Bucket Configuration:** Click **Create bucket.**

### Step 2: Configure the S3 Bucket for Static Website Hosting

1.Go to the newly created bucket.

2.**Properties** tab **→ Static website hosting.**

3.**Static website hosting:**

   - **Hosting type:** Select "Use this bucket to host a website".
    
   - **Index document:** Enter index.html.
    
   - **Error document:** Enter error.html (optional).
    
4.Save changes.


### Step 3: Set Bucket Policy for Public Access

1.Go to the **Permissions** tab.

2.**Bucket Policy**: Click **Edit** and paste the following JSON policy, replacing my-static-site-bucket with your bucket name:

        {
         '"Version":' "2012-10-17",
         "Statement": [
          {
            "Effect": "Allow",
            "Prinicipal": "*",
            "Action": "s3.GetObject",
            "resource": "arn:aws:s3:::my-static-site-bucket/*"
            }
           ]
          }
                   
3.Save changes.

### Step 4: Upload Your Static Site Files

1.**Click on the bucket name** to enter it.

2.**Upload:**
    Click **Upload.**
    
    Add your index.html, styles.css, and other files.
    
    Click Upload.
        
### Step 5: Set Up AWS CloudFront (Optional, but recommended for better performance)

1.Navigate to **CloudFront** and click **Create Distribution.**

2.**Select:** Get Started with the Web delivery method.

3.**Origin Settings:**

- **Origin Domain Name:** Enter your S3 bucket URL (e.g., my-static-site-bucket.s3.amazonaws.com).
  
4.**Default Cache Behavior Settings:** Adjust as needed.
  
5.**Distribution Settings:** Customize as needed.

6.**Create Distribution.**

### Step 6: Create a GitHub Repository

1.Go to GitHub and create a new repository.

2.Clone the repository to your local machine and add your static site files.
   
### Step 7: Set Up GitHub Actions for Deployment

1.In your repository, create a .github/workflows directory.

2.Create a deploy.yml file inside the .github/workflows directory.

### Example deploy.yml File:

name: Deploy to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1

    - name: Sync S3 bucket
      run: aws s3 sync . s3://my-static-site-bucket --delete
Step 8: Add AWS Credentials to GitHub Secrets
Go to your GitHub repository.
Settings → Secrets and variables → Actions.
New repository secret:
AWS_ACCESS_KEY_ID: Enter your AWS access key ID.
AWS_SECRET_ACCESS_KEY: Enter your AWS secret access key.
Step 9: Commit and Push Your Code
Commit your changes and push them to the main branch.
GitHub Actions will trigger the deployment workflow, and your static site will be deployed to the S3 bucket.
Step 10: Verify Deployment
Visit your S3 bucket URL or CloudFront URL (if configured) to see your static website live.
By following these steps, you'll have a fully automated process for deploying your static website to AWS using GitHub Actions.
