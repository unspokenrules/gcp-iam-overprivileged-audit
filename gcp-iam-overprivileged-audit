import argparse
import json
import subprocess
import csv

def get_iam_policy(project_id):
    cmd = ["gcloud", "projects", "get-iam-policy", project_id, "--format=json"]
    result = subprocess.run(cmd, capture_output=True, text=True, check=True)
    return json.loads(result.stdout)

def is_overprivileged(role):
    return role in ["roles/editor", "roles/owner", "roles/iam.serviceAccountUser"]

def audit_project(project_id):
    policy = get_iam_policy(project_id)
    results = []

    for binding in policy.get("bindings", []):
        role = binding["role"]
        if is_overprivileged(role):
            for member in binding["members"]:
                results.append({
                    "principal": member,
                    "role": role,
                    "recommendation": "Use a custom role or least privilege alternative"
                })
    return results

def save_report(data, filename="gcp-iam-overprivileged-audit.csv"):
    with open(filename, "w", newline="") as f:
        writer = csv.DictWriter(f, fieldnames=["principal", "role", "recommendation"])
        writer.writeheader()
        writer.writerows(data)

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--project", required=True, help="GCP project ID")
    args = parser.parse_args()

    audit_data = audit_project(args.project)
    save_report(audit_data)
    print(f"Audit complete. Report saved to gcp-iam-overprivileged-audit.csv")
