# Jenkins-to-ECR CI/CD Pipeline

A hands-on CI/CD pipeline that takes a Node.js application from source to a running
container image in AWS ECR, using a Jenkins Multibranch Pipeline on a self-managed EC2 host.

## What it does
- Automatically builds and tests a Node.js app on every branch push
- Builds a Docker image using a multi-stage Dockerfile
- Pushes the tagged image to AWS Elastic Container Registry (ECR)
- Runs entirely on a self-provisioned EC2 instance (no managed CI service)

## Tech stack
Jenkins (Multibranch Pipeline) · Docker (multi-stage builds) · AWS ECR · AWS EC2 · Git/GitHub

## What I built and debugged
- Installed and configured Jenkins from scratch on EC2, including systemd service management
- Debugged a Java version mismatch that was blocking Jenkins from starting, using `systemctl` and `journalctl` logs
- Found and fixed a missing Git installation on the EC2 host that was breaking pipeline checkouts
- Resolved GitHub API rate-limiting errors during webhook/checkout by switching to PAT-based credentials
- Wrote the multi-stage Dockerfile to keep the final image lean
- Configured the pipeline's ECR push step with correct IAM permissions and repo authentication

## How to run
1. Launch an EC2 instance and install Jenkins, Docker, and the AWS CLI
2. Configure AWS credentials on the instance (IAM role or PAT-based access)
3. Create a Multibranch Pipeline job in Jenkins pointing at this repo
4. Push to any branch — Jenkins will build, containerize, and push to ECR automatically

## Notes
This was built as a hands-on learning project to understand the full path from
CI trigger to a deployable container image, including the real infrastructure
debugging that doesn't show up in tutorials.
