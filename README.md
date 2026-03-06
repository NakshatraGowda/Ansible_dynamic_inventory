# Ansible Dynamic Inventory - AWS EC2 Nginx Deployment

Automated Nginx deployment to AWS EC2 instances using Ansible with dynamic inventory. Auto-discovers instances by tags and configures Nginx, Git, and application code.

## Prerequisites

- Python 3.6+, Ansible 2.9+, AWS CLI
- EC2 instances tagged with `Role: web` (running, Ubuntu)
- SSH key pair (`.pem` file)

## Quick Start

```bash
# Install dependencies
pip install ansible boto3 botocore
ansible-galaxy collection install amazon.aws

# Configure AWS
aws configure

# Update SSH key path in ansible.cfg and aws_ec2.yml
chmod 400 /path/to/key.pem

# Deploy
ansible-playbook nginx-deployment.yml
```

## Files

| File | Purpose |
|------|---------|
| `ansible.cfg` | Ansible settings (inventory, SSH key path) |
| `aws_ec2.yml` | Dynamic inventory config (regions, filters, tags) |
| `nginx-deployment.yml` | Main playbook (Nginx, Git, app deployment) |
| `templates/nginx.conf.j2` | Nginx configuration template |

## Configuration

**aws_ec2.yml** - Customize regions and filters:
```yaml
regions: [ap-southeast-2]
filters:
  tag:Role: web
  instance-state-name: running
```

**ansible.cfg** - Update SSH key path:
```ini
private_key_file = /path/to/awsEC2keypair.pem
```

## Common Commands

```bash
# Check connectivity
ansible all -m ping

# List instances
ansible-inventory --graph

# Dry run
ansible-playbook nginx-deployment.yml --check

# Verbose
ansible-playbook nginx-deployment.yml -vv

# Deploy to specific host
ansible-playbook nginx-deployment.yml --limit "ip-address"
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Private key not found | Check path in `ansible.cfg`/`aws_ec2.yml`, `chmod 400` |
| Permission denied (publickey) | Verify SSH key matches instances, security group allows port 22 |
| No instances found | Check instances tagged as `Role: web`, in running state |
| Import errors | `pip install --upgrade boto3 botocore` |
