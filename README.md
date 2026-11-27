# Deploy a Cron Job to Backup PostgreSQL to Amazon S3

This repo can be used to deploy a [Cron Job](https://render.com/docs/cronjobs) on [Render](https://render.com) to backup a PostgreSQL instance to Amazon S3.

Fork this repo and set the database name in `render.yaml` to your Render Postgres service.

## Configure AWS access

1. Create a private S3 backup bucket and record its name and AWS region.
2. In AWS IAM, register your Render workspace as an OIDC provider and create a backup role. For an existing bucket using S3-managed encryption, allow `s3:ListBucket` for the script's root listing and `s3:PutObject` and `s3:AbortMultipartUpload` on the dated backup keys (`YYYY/MM/DD/backup-HH-MM-SS.sql.gz`). [Render AWS OIDC setup](https://render.com/docs/oidc#aws-setup)
3. Deploy the Blueprint using the button below. Supply the bucket name as `S3_BUCKET_NAME`, its region as `AWS_REGION`, and the role ARN as `AWS_ROLE_ARN`. Scope the role's trust policy to the exact subject of the new cron job before its first scheduled run. A replacement cron job gets its own service ID.

For an existing Blueprint, set `AWS_ROLE_ARN` in the cron job's environment and build it. Render then supplies the token file to the AWS CLI. Before removing the service's static credentials, unset them in a cron shell and verify the assumed role and a synthetic upload. Use a temporary permission for a disposable test prefix and clean it up afterwards. [Render Blueprint secret prompts](https://render.com/docs/blueprint-spec#prompting-for-secret-values)

## Select the backup image versions

Set the corresponding OS and Postgres versions when you deploy.

| `POSTGRES_VERSION` | `ALPINE_VERSION` |
| :----------------: | :--------------: |
| 18                 | 3.23             |
| 17                 | 3.21             |
| 16                 | 3.20             |
| 15                 | 3.20             |
| 14                 | 3.20             |
| 13                 | 3.19             |
| 12                 | 3.18             |

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy)

