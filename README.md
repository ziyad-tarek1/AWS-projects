# AWS Nginx Web App Deployment

This guide walks through the process of setting up an Nginx-based web application, pushing it to Amazon Elastic Container Registry (ECR), and deploying it on AWS App Runner.

## Prerequisites

- [AWS CLI](https://aws.amazon.com/cli/) configured with appropriate IAM permissions.
- Docker installed and running on your local machine.

---

## Steps

### 1. Project Setup

1. Create a new project directory and navigate into it:
    ```bash
    mkdir aws-nginx-web-app
    cd aws-nginx-web-app
    ```

2. Create a `Dockerfile` with the following content:
    ```Dockerfile
    FROM --platform=linux/amd64 nginx:latest
    WORKDIR /usr/share/nginx/html
    COPY index.html index.html
    ```

3. Create an `index.html` file in the project directory with your desired HTML content.

### 2. Build and Test Docker Image

1. Build the Docker image locally:
    ```bash
    docker build -t aws-nginx-web-app .
    ```

### 3. Create an ECR Repository

1. Create a new ECR repository for your application:
    ```bash
    aws ecr create-repository --repository-name aws-nginx-web-app --region us-east-1
    ```

2. Confirm the repository was created successfully:
    ```bash
    aws ecr describe-repositories --region us-east-1
    ```

### 4. Push the Docker Image to ECR

1. Run the deployment script or follow the manual push commands provided by ECR:
    ```bash
    ./aws-ECR.sh
    ```

    Alternatively, you can select your repository in the AWS Console and follow the “View push commands” instructions to push your Docker image.

### 5. Create an AWS App Runner Service

1. Go to **AWS App Runner** in the AWS Console.

2. In the **Source and deployment** section:
   - Leave the default selections for **Repository type** and **Provider**.
   - For **Container image URI**, select **Browse**.

3. In the pop-up window:
   - For **Image repository**, select `aws-nginx-web-app`.
   - Choose **Continue**.

4. In the **Deployment settings** section:
   - For **ECR access role**, select **Create new service role**.
   - Choose **Next**.

5. On the **Configure service** page:
   - For **Service name**, enter `nginx-web-app-service`.
   - Change the **Port** to `80`.
   - Leave the rest of the settings as default and select **Next**.

6. On the **Review and create** page, review all inputs, then choose **Create & deploy**.

7. Wait a few minutes for the service to be deployed. You can view the event logs for progress.

---

## Verification

Once the deployment is complete, you can access your Nginx web application using the URL provided by App Runner.

---

## Cleanup

To delete the ECR repository and App Runner service when you're finished:
1. Go to **ECR** in the AWS Console and delete the `aws-nginx-web-app` repository.
2. Go to **App Runner** and delete the `nginx-web-app-service`.

---

## Notes

- Make sure your AWS CLI is configured with the necessary permissions for creating ECR repositories and App Runner services.
- Update your AWS CLI region as needed in the commands.

---

This completes the setup and deployment of your Nginx web app to AWS App Runner!
