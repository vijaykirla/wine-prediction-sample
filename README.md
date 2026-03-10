    
### Data Version Control with AWS S3 & GitHub

---

## Overview

You will use **Data Version Control (DVC)** to track your dataset and **AWS S3** as remote storage, while keeping metadata and configuration in GitHub.

---

## Prerequisites

- Python 3.8 or higher installed
- Git installed and configured with a GitHub account
- An active AWS account with permissions to create S3 buckets and IAM access keys
- AWS CLI installed (see Step 13)

---

## Project Structure

project directory will look like this:

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


