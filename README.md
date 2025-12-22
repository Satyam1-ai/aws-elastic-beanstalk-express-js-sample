# AWS Elastic Beanstalk Node.js Sample – My Adaptation

This repository is a **fork** of the official  
[AWS Elastic Beanstalk Express.js sample](https://github.com/aws-samples/aws-elastic-beanstalk-express-js-sample).

## What I Did

- Used the sample as a base to understand deployment of Node.js apps on **AWS Elastic Beanstalk**.
- Modified application configuration and messages for my environment and coursework.
- Integrated with my **Jenkins + Docker-in-Docker** setup:
  - Added/updated `buildspec.yml` for **AWS CodeBuild**.
  - Used a `Jenkinsfile` in a separate repo to build, test, scan (Snyk), and deploy this app.
