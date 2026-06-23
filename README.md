# aws-projects

This repository serves as a portfolio for AWS architectural experiments, cloud compute case studies, and safe account operational tools. It is split into two primary directories: one focused on compute selection case studies, and the other on a governed operator toolkit.

---

## Repository Structure

```text
aws-projects/
├── aws-projects/         # Cloud compute selection & migration case studies
│   ├── Project-1-EC2-by-necessity/
│   ├── Project-2-Lambda-daily-computation/
│   └── Project-3-Orchestrated-System-ECS-Fargate/
└── aws-tools/            # Governed operator toolkit for AWS account management
    ├── aws_cleaner.py
    ├── aws_ec2_manager.py
    ├── aws_health_check.py
    ├── aws_iam_manager.py
    ├── aws_s3_manager.py
    └── aws_shutdown.py
```

---

## Directory Summaries

### 1. [aws-projects](./aws-projects/)
A progression of three projects demonstrating compute selection decisions based strictly on workload requirements and execution constraints:
* **Project 1: EC2 by Necessity** – A case study on why EC2 was selected over Lambda and ECS due to long-running, stateful connection requirements.
* **Project 2: Lambda Daily Computation** – A continuation of Project 1 showing how the workload was redesigned to externalize state and make AWS Lambda the correct compute choice.
* **Project 3: Orchestrated System (ECS/Fargate)** – Running time-bound batch and streaming tasks that require independent compute containers but exceed Lambda's execution limits.

### 2. [aws-tools](./aws-tools/)
A suite of operator-first, conservative Python scripts designed to safely manage and clean up AWS accounts. 
* **`aws_health_check.py`** – Account and region diagnostics (Read-Only).
* **`aws_cleaner.py`** – Hygiene analysis and resource age tracking (Read-Only).
* **`aws_iam_manager.py`** – Safe structural visibility into IAM blast radius (Read-Only).
* **`aws_ec2_manager.py`** – Safe, manual-confirmation EC2 operations.
* **`aws_s3_manager.py`** – Guarded S3 bucket inspection, upload, and deletion.
* **`aws_shutdown.py`** – Multi-region final account cleanup protocol to guarantee zero-cost state.
