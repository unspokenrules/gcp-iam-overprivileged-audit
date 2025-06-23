# GCP IAM Over-Privileged Account Detection

This project identifies IAM principals (users or service accounts) that have broader permissions than necessary across GCP projects. It uses the Cloud Asset Inventory API to analyze IAM bindings, generates a CSV report, and suggests tighter, least-privilege alternatives.

## 💡 Features

- Scan a project or entire GCP org
- Detect broad roles (e.g. `roles/editor`, `roles/owner`,`roles/iam.serviceAccountUser`)
- List users/service accounts with excessive permissions
- Output to CSV with recommendations

## 📦 GCP Services Used

- Cloud Asset Inventory API
- IAM Policy Analysis
- gcloud SDK

## 🛠️ Prerequisites

- Python 3.8+
- `gcloud` CLI configured
- Cloud Asset API enabled

```bash
gcloud services enable cloudasset.googleapis.com
```

## 🚀 Usage

```bash
git clone https://github.com/yourname/gcp-iam-overprivileged-audit.git
cd gcp-iam-overprivileged-audit
pip install -r requirements.txt
python gcp-iam-overprivileged-audit.py --project my-gcp-project-id
```

## 📂 Output

| Principal         | Role             | Recommendation          |
|------------------|------------------|--------------------------|
| svc-123@...      | roles/editor     | Replace with custom role|
| user@domain.com  | roles/owner      | Audit, then downgrade    |

## 📌 Why This Matters

Over-privileged accounts are a top cloud security risk. This tool provides actionable insights to implement least-privilege in your GCP org.
