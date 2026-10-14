# AWS EC2–IAM–S3 Integration Lab

## Overview
A hands-on AWS Solutions Architect lab demonstrating secure access from an Amazon Linux 2023 EC2 instance to an Amazon S3 bucket using an IAM instance role and AWS CLI, without manually configuring long-term access keys.

## AWS Services
- Amazon EC2
- AWS Identity and Access Management (IAM)
- Amazon S3
- AWS Security Token Service (STS)

## Architecture
Amazon EC2 → IAM role credentials → Amazon S3

The EC2 instance receives temporary credentials through its attached IAM role. AWS CLI uses those credentials to make authorised S3 API requests.

## Implementation
1. Launched an Amazon Linux 2023 EC2 instance.
2. Created an S3 bucket.
3. Created an IAM role for EC2 with S3 permissions.
4. Attached the role to the correct EC2 instance.
5. Verified the assumed-role identity with AWS STS.
6. Uploaded and downloaded a test file using AWS CLI.
7. Verified file integrity using SHA-256.

## AWS CLI Commands
```bash
aws --version
aws sts get-caller-identity
aws s3 ls s3://YOUR-BUCKET-NAME/
echo 'Hello AWS!' > aws-lab-test.txt
aws s3 cp aws-lab-test.txt s3://YOUR-BUCKET-NAME/
aws s3 cp s3://YOUR-BUCKET-NAME/aws-lab-test.txt downloaded-test.txt
sha256sum aws-lab-test.txt downloaded-test.txt
```

## Results
- IAM role authentication: successful
- S3 bucket listing: successful
- S3 object upload: successful
- S3 object download: successful
- SHA-256 hashes: identical

## Troubleshooting
Initially, AWS CLI reported `NoCredentials` because the IAM role had been attached to a different EC2 instance. Attaching the role to the correct instance resolved the issue. An initial upload also failed because the local file had not yet been created; creating it before running `aws s3 cp` resolved that error.

## Security Considerations
- Used an IAM role rather than storing static AWS access keys.
- Temporary credentials are supplied through EC2 instance metadata.
- IAM permissions should follow least privilege.
- Do not commit AWS credentials, secrets, or sensitive configuration files.

## Future Improvements
- Restrict IAM permissions to the required bucket and actions.
- Add an architecture diagram and sanitised screenshots.
- Explore S3 versioning, encryption, and monitoring.

## Skills Demonstrated
AWS EC2 · Amazon S3 · IAM · AWS CLI · Linux · Cloud Security · Troubleshooting · SHA-256 Integrity Verification
