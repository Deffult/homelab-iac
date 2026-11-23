#  Homelab as Code — v14.0

> **Author:** Ahmed A. Akmal (Mr.Deffult)
> **Version:** 14.0.0 · **License:** MIT

Full infrastructure built from code: provision → harden → deploy → monitor → backup.
One command up, one command down.

![CI](https://img.shields.io/badge/CI-GitHub%20Actions-2088FF?logo=githubactions&logoColor=white)
![Terraform](https://img.shields.io/badge/IaC-Terraform-7B42BC?logo=terraform&logoColor=white)
![Ansible](https://img.shields.io/badge/Config-Ansible-EE0000?logo=ansible&logoColor=white)
![Docker](https://img.shields.io/badge/Containers-Docker-2496ED?logo=docker&logoColor=white)
![Version](https://img.shields.io/badge/version-14.0.0-success)

##  Features
| Layer | Tool | What it does |
|---|---|---|
| Provisioning | **Terraform** | Ubuntu 24.04 VM on AWS free tier + security group + key pair |
| Hardening | **Ansible** | SSH keys only, UFW, fail2ban, unattended-upgrades, sysctl |
| Services | **Docker Compose** | Nginx, Gitea, Vaultwarden |
| Monitoring | **Prometheus + Grafana + Alertmanager** | Metrics, dashboards, Telegram alerts |
| Backup | **Restic / Bash** | Encrypted, rotated, cron-scheduled |
| CI/CD | **GitHub Actions** | terraform validate, yamllint, ansible-lint, shellcheck |

##  Architecture
See [`docs/architecture.md`](docs/architecture.md).

##  Quick Start
```bash
git clone https://github.com/Deffult/homelab-iac.git && cd homelab-iac

# 1) Provision
cd terraform && terraform init && terraform apply -var="ssh_public_key=$(cat ~/.ssh/id_ed25519.pub)"

# 2) Configure (put the output IP in ansible/inventory.ini)
cd ../ansible && ansible-playbook playbook.yml

# 3) Services: Grafana /grafana/ · Gitea /git/ · Vaultwarden / (all behind Nginx)
```

##  Security notes
- Root login & password auth disabled
- Firewall: only 22/80/443 open
- Secrets go in `.env` (git-ignored), never commit them
- Set Terraform `allowed_cidr` to your own IP (`x.x.x.x/32`)

##  Backup
```bash
sudo ./scripts/backup.sh          # manual
# cron (installed by Ansible): daily 03:00
```
Create the restic password file first: `echo 'strong-pass' | sudo tee /root/.restic_pass && sudo chmod 600 /root/.restic_pass`

##  Screenshots
Add your Grafana screenshots in `docs/` and reference them here.

## 🗺️ Roadmap
- [ ] WireGuard VPN role
- [ ] Loki log aggregation
- [ ] Ansible Vault for secrets

---
**Ahmed A. Akmal (Mr.Deffult)** · v14.0.0
