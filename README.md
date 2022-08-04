<div align="center">

<img src="https://github.com/ilanKushnir/kws-cluster/blob/main/docs/images/kws-k8sathome.png?raw=true" align="center" height="144px"/>

# KWS Cluster
[![k3s](https://img.shields.io/badge/k3s-v1.23-brightgreen?style=for-the-badge&logo=kubernetes&logoColor=white)](https://k3s.io/)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white&style=for-the-badge)](https://github.com/pre-commit/pre-commit)
[![Lines of code](https://img.shields.io/tokei/lines/github/ilanKushnir/kws-cluster?style=for-the-badge&color=brightgreen&label=lines&logo=codefactor&logoColor=white)](https://github.com/onedr0p/home-ops/graphs/contributors)
</div>
<br/>

## 📓 Overview
Kush-Web-Services is my Homelab GitOps and IaC project. This is the second verison of the cluster which uses tools like: [Ansible](https://www.ansible.com/), [Terraform](https://www.terraform.io/), [Kubernetes](https://kubernetes.io/), [Flux](https://github.com/fluxcd/flux2) and [GitHub](https://github.com/).
My main purpose was to practice K8S, GitOps and RPIs and to build my own infra for deploying my own staff :)

<div align="center">
<img src="https://github.com/ilanKushnir/kws-cluster/blob/main/docs/images/hardware/Screen%20Shot%202022-04-29%20at%201.26.54.png?raw=true" align="center" width="400px"/>
</div>
<br/>

## ⚡️ Tech Stack
* [Ansible] - Server configuration
* [Cert-Manager] - SSL certificates management
* [Cloudflare] - Domain and DNS management
* [External-DNS] - SOON... (Syncing DNS records with CF)
* [FluxCD] - My Git repo and cluster syncing
* [Helm] - Package manager for Kubernetes
* [Kubernetes] - Container orchestration
* [Longhorn] - Distributed block storage for peristent storage
* [MetalLB] - Load Balancer for bare metal Kubernetes clusters
* [SOPS] - Encrypted secrets in Git
* [Terraform] - VM provisioning

<br/>

## 📂 Repository structure
In the `/cluster` dir you'll find the following:
- **base**: directory is the entrypoint to [Flux](https://github.com/fluxcd/flux2).
- **crds**: directory contains custom resource definitions (CRDs) that need to exist globally in your cluster before anything else exists.
- **core**: directory (depends on **crds**) are important infrastructure applications (grouped by namespace) that should never be pruned by [Flux](https://github.com/fluxcd/flux2).
- **apps**: directory (depends on **core**) is where your common applications (grouped by namespace) could be placed, [Flux](https://github.com/fluxcd/flux2) will prune resources here if they are not tracked by Git anymore.
```
./cluster
├── ./apps
├── ./base
├── ./core
└── ./crds
```


### 💻 Deployed services
Explore the `/cluster/apps` folder, you will see cluster's namespaces with all the apps inside.
* **dev** - self hosted dev tools
* **media** - a full media server (Plex, Sonarr, Radarr, qBittorrent, and more!)
* **monitoring** - the known monitoring stack (Grafana, Prometheus, Alerts Manager)
* **networking** - mainly Traefik ingress manager and cloudflare
* and there are many more to come...

**some missing charts are deployed via my helm repo: [kws-charts]**

<br/>

## ⚙️ Hardware
| Device                   | Count | OS & Data Disk Size  | Ram   | Operating System   | Purpose                         |
|--------------------------|-------|----------------------|-------|--------------------|---------------------------------|
| Raspberry Pi 4b          | 1     | 512GB SSD (longhorn) | 8GB   | Ubuntu 22.04       | Kubernetes (k3s) Masters        |
| Raspberry Pi 4b          | 2     | 512GB SSD (longhorn) | 8GB   | Ubuntu 22.04       | Kubernetes (k3s) Workers        |
| Raspberry Pi 4b          | 1     | 512GB SSD            | 8GB   | Raspi OS 64GB      | Monitoring device               |
| Raspberry Pi 4b          | 1     | 2TB SSD              | 8GB   | OMV                | NAS - Network Attached Server   |
| Raspberry Pi Zero 2W     | 1     | N/A                  | 512MB | Raspi OS lite 32GB | Home DNS Server - PiHole        |
| Sandisk Extreme SSD      | 3     | 500GB                | N/A   | N/A                | Distributed persistent storage  |
| Sandisk Extreme SSD      | 1     | 500GB                | N/A   | N/A                | Monitor storage drive           |
| Sandisk Extreme SSD      | 1     | 2TB                  | N/A   | N/A                | NAS storage drive               |
| WD - My Book HDD         | 1     | 3TB                  | N/A   | N/A                | Backup external HDD for NAS     |
| TP-Link TL-SG1005P V2    | 1     | N/A                  | N/A   | N/A                | Network switch with PoE support |
| APC Back 1200VA/650Watts | 1     | N/A                  | N/A   | N/A                | UPS                             |
| SOON - Router            | 1     |                      |       |                    | 1000 mbps                       |

<br/>

## ⛵️ Installation
As an educative project I suggest starting with the manual work, explore Kubernetes, Raspberry PIs, Docker's and build one of your own from scratch.
For those who wants to go to the next stage - yes! you can install the same cluster on your machines! I'll provide the original [Kubernetes @Home](https://github.com/k8s-at-home/)'s instructions along with my additions in the `/docs` folder.

<br/>
<div align="center">
<img src="https://github.com/ilanKushnir/kws-cluster/blob/main/docs/images/hardware/Screen%20Shot%202022-04-29%20at%201.28.32.png?raw=true" align="center" width="600px"/>
</div>

<br/>

## ☠️️ Disaster recovery plan
There are 3 types of backups should be done on the cluster:
* Longhorn PV/PVCs backups
* ETCD state backup
* NAS data backup

For more details go to `/docs/DR-plan.md`

### Backups flow
<div align="center">
<img src="https://github.com/ilanKushnir/kws-cluster/blob/main/docs/diagrams/backups-flow.drawio.png?raw=true" align="center" width="600"/>
</div>
<br/>

<br/>

## 🤝 Graditude
Thanks to the great supportive community at [Kubernetes @Home](https://github.com/k8s-at-home/).
Most of the support and inspiration for my cluster came from [onedr0p](https://github.com/onedr0p/) and from his [super cluster](https://github.com/onedr0p/home-ops).

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)
[Helm]: <https://helm.sh/>
[SOPS]: <https://github.com/mozilla/sops>
[FluxCD]: <https://github.com/fluxcd/flux2>
[Ansible]: <https://www.ansible.com/>
[MetalLB]: <https://metallb.universe.tf/>
[Longhorn]: <https://longhorn.io/>
[Terraform]: <https://www.terraform.io/>
[Cloudflare]: <https://www.cloudflare.com/>
[Kubernetes]: <https://kubernetes.io/>
[External-DNS]: <https://github.com/kubernetes-sigs/external-dns>
[Cert-Manager]: <https://cert-manager.io/docs/>

[kws-charts]: <https://github.com/ilanKushnir/kws-charts>
