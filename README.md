## convert all tabs to spaces in the inventory script file 
sed -i 's/\t/    /g' /opt/ansible/inventory/gcp_swarm_inventory.py


## Giving execute permission to the inventory script
chmod +x /opt/ansible/inventory/gcp_swarm_inventory.py

## Test the inventory script
ansible-inventory -i /opt/ansible/inventory/gcp_swarm_inventory.py --list

## confirm ansible can connect to the manager node
ansible -i /opt/ansible/inventory/gcp_swarm_inventory.py manager -m ping
---

## artifact registry permissions (GAR)

1) Go to service accounts in GCP console 
2) Select the service account used for ansible or create a new one
3) click permissions tab
4) click "Grant Access"
5) add the following roles:
   - Artifact Registry writer
   - Viewer
   - compute admin
   - compute instance admin
   - compute load balancer admin
   - compute network admin
   - storage object admin
   - Storage Object Viewer
   - Compute Viewer
   - Service Account User
6) Save


### to check if the service account email being used on the instance, run the following command on the instance terminal:

```bash
curl -H "Metadata-Flavor: Google" \
http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/email
```
---
## list of service accounts associated with the instance 

```bash
curl -H "Metadata-Flavor: Google" \
http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/
```

## Roles needed for Terraform Service Account
| Purpose                                 | Role                                   |
| --------------------------------------- | -------------------------------------- |
| Push images to GAR                      | `roles/artifactregistry.writer`        |
| Create/modify VM instances              | `roles/compute.instanceAdmin.v1`       |
| Manage VPC, subnet, firewall rules      | `roles/compute.networkAdmin`           |
| Allow startup scripts, metadata updates | `roles/compute.admin`                  |
| Allow Terraform to impersonate SA       | `roles/iam.serviceAccountUser`         |
| Allow Terraform to generate tokens      | `roles/iam.serviceAccountTokenCreator` |
| Access GCS bucket backend               | `roles/storage.admin`                  |
| Generic read permission                 | `roles/viewer`                         |
