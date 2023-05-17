# OpenShift APIs for Data Protection (OADP) Demo

This repo contains supporting files for OADP demo.

## Troubleshooting commands

Command to download restore log files from s3 bucket:
```
aws s3 mv s3://oadp/demo/restores/restore/restore-restore-results.gz .
```