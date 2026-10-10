# Troubleshooting Notes

## Private-subnet package installation
- **Issue:** `dnf install httpd` could not reach public package repositories from backend instances.
- **Cause:** The backend private subnets had no NAT Gateway or other outbound path to the repositories.
- **Resolution:** Created a custom AMI with Apache already installed and updated the backend Launch Template to use it.
- **Trade-off:** The backend instance served the same sample page as the web instance; this was an infrastructure demo, not a complete application.

## Backend target group
- **Issue:** Backend targets did not appear as expected.
- **Cause:** The backend ASG was initially not associated with `backend-target-group`.
- **Resolution:** Associated the target group with the ASG, updated the Launch Template, and replaced old instances.
- **Result:** Two healthy backend targets were reported.

## Web server provisioning
- **Issue:** Older web instances did not have the expected Apache configuration.
- **Resolution:** Added Apache installation/startup commands to the web Launch Template User Data and replaced old instances.
- **Validation:** Apache was active and `curl http://localhost` returned `Three-Tier Web Presentation Layer`.

## Connectivity
- Public ALB forwarded to `web-target-group`.
- Web tier `curl` to the internal ALB DNS returned an HTTP response.
- RDS Security Group allowed TCP 3306 from `backend-sg`.
- A live database login/query was not completed; don't describe the DB connection as end-to-end tested.
