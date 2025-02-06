- [CICD Pipeline Guide](#cicd-pipeline-guide)
  - [Overview](#overview)
  - [1. CICD Pipeline Triggering Methods](#1-cicd-pipeline-triggering-methods)
  - [2. CICD Pipeline Components](#2-cicd-pipeline-components)
  - [3. Deployment Environments](#3-deployment-environments)
  - [4. Webhook and Jenkins Integration](#4-webhook-and-jenkins-integration)
  - [5. Benefits of CICD Pipelines](#5-benefits-of-cicd-pipelines)
  - [6. Jenkins Setup and Usage](#6-jenkins-setup-and-usage)
    - [**Creating a New Jenkins Project**](#creating-a-new-jenkins-project)
    - [**Managing Build History**](#managing-build-history)
  - [7. Creating and Running Pipelines](#7-creating-and-running-pipelines)
    - [**Basic Jenkins Pipeline (Fetching Date and Time) Manually**](#basic-jenkins-pipeline-fetching-date-and-time-manually)
    - [**Running a Pipeline Manually**](#running-a-pipeline-manually)
  - [8. Multistage Pipelines (Manual Setup)](#8-multistage-pipelines-manual-setup)
    - [**Setting Up a Multi-Stage Pipeline Manually**](#setting-up-a-multi-stage-pipeline-manually)
    - [**Step-by-Step Multi-Stage Job Execution**](#step-by-step-multi-stage-job-execution)
  - [Conclusion](#conclusion)

# CICD Pipeline Guide

## Overview

This guide provides a comprehensive understanding of CICD pipelines, including triggering methods, components, deployment environments, and Jenkins integration. It covers best practices and step-by-step instructions for setting up and using Jenkins for CICD.

---

## 1. CICD Pipeline Triggering Methods

- **GitHub Push Triggers**: Automatically triggers a pipeline when new code is pushed to the repository.
- **Manual Triggers**: Jenkins dashboard allows users to manually start a pipeline.
- **Webhooks**: Set up to notify Jenkins of repository changes, initiating an automated build.

---

## 2. CICD Pipeline Components

- **Continuous Integration (CI)**: Automates the build and testing process.
- **Continuous Deployment (CD)**: Deploys the tested code to staging or production environments.

---

## 3. Deployment Environments

- **Test Environment**: Used for running unit and integration tests.
- **Staging Environment**: A pre-production environment for testing in conditions similar to production.
- **Production Environment**: The live environment where end users interact with the deployed application.
- **Server Deployment Options**:
  - Deploy on cloud instances (e.g., AWS EC2, Azure VMs).
  - Use containerized solutions (Docker, Kubernetes).

---

## 4. Webhook and Jenkins Integration

- **Webhooks Role**: Notify Jenkins of changes in a repository to trigger builds.
- **Jenkins Integration Steps**:
  1. Configure webhook in GitHub/GitLab.
  2. Set up Jenkins to listen for webhook notifications.
  3. Automatically trigger pipeline upon receiving notifications.

---

## 5. Benefits of CICD Pipelines

- **Automation**: Reduces manual intervention and accelerates development cycles.
- **Efficiency**: Speeds up software delivery by automating build, test, and deployment steps.
- **Risk Reduction**: Small, frequent updates minimize the risk of deployment failures.
- **Faster Rollbacks**: Quick reverts in case of issues.

---

## 6. Jenkins Setup and Usage

### **Creating a New Jenkins Project**

1. Go to **Jenkins Dashboard**.
2. Click **"New Item"** → **"Pipeline"**.
3. Define project name and click **"OK"**.
4. Configure build steps (script execution, tests, deployments).
5. Save and trigger the pipeline.

### **Managing Build History**

- Configure Jenkins to discard old builds to optimize storage and maintain performance.

---

## 7. Creating and Running Pipelines

### **Basic Jenkins Pipeline (Fetching Date and Time) Manually**

1. Open Jenkins Dashboard.
2. Click on **"New Item"** and create a **"Freestyle Project"**.
3. In the **"Build"** section, click **"Add build step"** → **"Execute shell"**.
4. Enter the following command:
   ```sh
   date
   ```
5. Click **"Save"** and then **"Build Now"**.
6. Check the **Console Output** to see the execution results.

### **Running a Pipeline Manually**

1. Go to the Jenkins job.
2. Click **"Build Now"**.
3. Check Console Output for execution logs.

![date ](<images/Screenshot 2025-02-05 115838.png>)

---

## 8. Multistage Pipelines (Manual Setup)

### **Setting Up a Multi-Stage Pipeline Manually**

1. **Create multiple jobs** in Jenkins.
2. **Configure the first job** to trigger the second job:
   - Go to **Post-build Actions** → **"Build other projects"**.
   - Enter the next job name.
   - Ensure no extra spaces or commas.
3. **Save and run the first job** to automatically trigger the next job.

### **Step-by-Step Multi-Stage Job Execution**

1. Create **Job 1 (System Info Fetcher)**:
   - Add a build step → **"Execute shell"**.
   - Enter the command:
     ```sh
     uname -a
     ```
   - Save and execute.
2. Create **Job 2 (Date Fetcher)**:
   - Add a build step → **"Execute shell"**.
   - Enter the command:
     ```sh
     date
     ```
   - Save the job.
3. Link **Job 1 to Job 2**:
   - Open **Job 1** → **Configure**.
   - In **Post-build Actions**, select **"Build other projects"**.
   - Enter **Job 2** name and save.
4. Run **Job 1** and verify that **Job 2** executes automatically after Job 1 completes.

---

## Conclusion

By following this guide, you can set up and manage CICD pipelines effectively in Jenkins, automating software builds, tests, and deployments. Multi-stage pipelines enable job dependencies, ensuring a streamlined and efficient workflow.
