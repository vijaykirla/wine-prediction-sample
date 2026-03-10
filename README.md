1. create a new directory with name "wine-prediction" in your local environment
2. in the directory, create a folder named "data"
3. inside the "data" folder create a dataset "wine-quality-sample" in csv format
4. create python virtual environment using command prompt "python -m venv .venv"
5. activate the environment ".venv\Scripts\activate.bat"
6. install data version control (dvc) using the command "pip install dvc"
7. intitialize dvc using "dvc init"
8. add the dataset to dvc using "dvc add data/wine-quality-sample.csv"
9. after executing the above the command metadata file with the name "wine-quality-sample.csv.dvc" will be created in "data" folder
10. store dataset in aws s3 bucket and dvc file in github
11. create s3 bucket in aws, create access key in security credentials
12. add the s3 bucket as remote using "dvc remote add -d wineremote s3://bucket-name"
13. install aws cli to configure s3 bucket
14. execute "aws configure" and enter the access key id, secret access key, default region name
15. install "pip install dvc_s3"
17. add "wine-quality-sample.csv.dvc" to git
18. git commit
19. execute "dvc push" to push dataset to s3 bucket
20. finally the .dvc folder which contains config and metadata file "wine-quality-sample.csv.dvc" in the data folder should be committed to github and the dataset to s3 bucket
    
# Wine Quality Prediction — DVC Setup Guide
### Data Version Control with AWS S3 & GitHub

---

## Overview

This guide walks you through setting up a versioned machine learning data pipeline for a wine quality prediction project. You will use **Data Version Control (DVC)** to track your dataset and **AWS S3** as remote storage, while keeping metadata and configuration in GitHub.

By the end of this setup, your dataset will be stored in S3, and the lightweight `.dvc` metadata files will be committed to GitHub — enabling reproducible, version-controlled ML workflows.

---

## Prerequisites

- Python 3.8 or higher installed
- Git installed and configured with a GitHub account
- An active AWS account with permissions to create S3 buckets and IAM access keys
- AWS CLI installed (see Step 13)
- Basic familiarity with the command line / terminal

---

## Project Structure

After completing this guide, your project directory will look like this:

```
wine-prediction/
├── .dvc/                                    ← DVC config and cache (commit to Git)
│   └── config                               ← Remote storage config
├── .venv/                                   ← Python virtual environment (do NOT commit)
├── data/
│   ├── wine-quality-sample.csv              ← Raw dataset (stored in S3, NOT Git)
│   └── wine-quality-sample.csv.dvc          ← DVC metadata file (commit to Git)
└── .gitignore                               ← Auto-updated by DVC
```

---

## Step-by-Step Setup

### Step 1 — Create Project Directory

```bash
mkdir wine-prediction
cd wine-prediction
```

Creates the root project folder on your local machine.

---

### Step 2 — Create Data Folder

```bash
mkdir data
```

This folder will store raw datasets and DVC-tracked files.

---

### Step 3 — Create Sample Dataset

Inside the `data/` folder, create a CSV file named `wine-quality-sample.csv` with your wine quality data.

---

### Step 4 — Create Virtual Environment

```bash
python -m venv .venv
```

Isolates project dependencies from the global Python environment.

---

### Step 5 — Activate Virtual Environment

**Windows:**
```bat
.venv\Scripts\activate.bat
```

**macOS / Linux:**
```bash
source .venv/bin/activate
```

---

### Step 6 — Install DVC

```bash
pip install dvc
```

Installs Data Version Control for managing and versioning datasets.

---

### Step 7 — Initialize DVC

```bash
dvc init
```

Run this inside the project directory. It creates a `.dvc/` folder with config files. Commit the generated files to Git:

```bash
git add .dvc .gitignore
git commit -m "Initialize DVC"
```

---

### Step 8 — Add Dataset to DVC

```bash
dvc add data/wine-quality-sample.csv
```

This tracks the dataset with DVC and auto-generates a `.dvc` metadata file.

---

### Step 9 — Verify Metadata File

After running the above command, a metadata file is automatically created at:

```
data/wine-quality-sample.csv.dvc
```

> ⚠️ Do **not** edit this file manually. It is managed by DVC.

---

### Step 10 — Create AWS S3 Bucket

Log in to the **AWS Management Console**, navigate to S3, and create a new bucket. Note the bucket name — you will need it in Step 12.

---

### Step 11 — Create AWS Access Key

In the AWS Console, go to **IAM → Security Credentials → Create Access Key**. Save both the **Access Key ID** and **Secret Access Key** securely.

> ⚠️ Never commit your AWS credentials to Git.

---

### Step 12 — Add S3 as DVC Remote

```bash
dvc remote add -d wineremote s3://your-bucket-name
```

Replace `your-bucket-name` with the actual name of your S3 bucket.

---

### Step 13 — Install AWS CLI

Download and install the AWS CLI from: https://aws.amazon.com/cli/

Verify the installation:
```bash
aws --version
```

---

### Step 14 — Configure AWS CLI

```bash
aws configure
```

Enter the following when prompted:

| Field | Value |
|---|---|
| AWS Access Key ID | From Step 11 |
| AWS Secret Access Key | From Step 11 |
| Default region name | e.g. `us-east-1` |
| Default output format | `json` |

---

### Step 15 — Install DVC S3 Plugin

```bash
pip install dvc-s3
```

> ⚠️ The correct package name is `dvc-s3` (with a hyphen). The original README listed `dvc_s3` — both may resolve, but `dvc-s3` is the canonical PyPI name.

---

### Step 16 — Stage Files for Git

```bash
git add data/wine-quality-sample.csv.dvc .gitignore
```

Always stage `.gitignore` alongside the `.dvc` file — DVC auto-updates it to exclude the raw CSV from Git.

---

### Step 17 — Commit to Git

```bash
git commit -m "Add wine dataset DVC tracking"
```

---

### Step 18 — Push Dataset to S3

```bash
dvc push
```

Uploads the actual CSV dataset to your configured S3 remote.

---

### Step 19 — Push Metadata to GitHub

```bash
git push
```

Pushes the `.dvc/` folder, metadata files, and `.gitignore` to GitHub.

---

## What Gets Stored Where

| Item | GitHub | AWS S3 |
|---|---|---|
| `wine-quality-sample.csv` (dataset) | ❌ Not stored | ✅ Stored here |
| `wine-quality-sample.csv.dvc` (metadata) | ✅ Committed | ❌ Not stored |
| `.dvc/config` (remote config) | ✅ Committed | ❌ Not stored |
| `.gitignore` (auto-updated) | ✅ Committed | ❌ Not stored |
| `.venv/` (virtual environment) | ❌ Not committed | ❌ Not stored |

---

## Common Mistakes to Avoid

- **Missing step:** The original README skipped Step 16 — numbering jumped from 15 to 17. This is corrected above (19 steps total).
- **Wrong package name:** Use `dvc-s3`, not `dvc_s3`.
- **Forgetting `.gitignore`:** Always stage it with the `.dvc` file after running `dvc add`.
- **Wrong activation command:** `.venv\Scripts\activate.bat` is Windows-only. Use `source .venv/bin/activate` on macOS/Linux.
- **Credential exposure:** Never commit AWS keys. Use `aws configure` or environment variables.
- **Order of operations:** Always configure AWS CLI (Step 14) before running `dvc push` (Step 18).
