<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f2027,50:203a43,100:2c5364&height=220&section=header&text=Architecture%20Gallery&fontSize=54&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=How%20all%20my%20DevOps%20%26%20Cloud%20projects%20fit%20together&descAlignY=62&descSize=18" width="100%" />

<a href="https://git.io/typing-svg">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3200&pause=900&color=38BDF8&center=true&vCenter=true&width=700&lines=5+projects.+1+DevOps+story.;Terraform+%E2%86%92+EKS+%E2%86%92+GitOps+%E2%86%92+CI%2FCD+%E2%86%92+Observability;Every+diagram+below+is+drawn+from+the+real+repos" alt="Typing SVG" />
</a>

<br/>

![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![AWS](https://img.shields.io/badge/AWS_EKS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![ArgoCD](https://img.shields.io/badge/Argo_CD-EF7B4D?style=for-the-badge&logo=argo&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)

**By [Sibi Karthik](https://github.com/cibi0zh)** · [LinkedIn](https://www.linkedin.com/in/sibi-karthik-219b1837a/) · [ORCID](https://orcid.org/0009-0003-0007-1968)

</div>

---

## 🧭 Jump to a project

| # | Project | Focus | Stack |
|:-:|---|---|---|
| 0 | [🗺️ The big picture](#big-picture) | How everything connects | All of the below |
| 1 | [☁️ Enterprise Cloud Platform](#platform) | Multi-AZ AWS + EKS with modular Terraform | `Terraform` `EKS` `ALB` `ASG` `Helm` |
| 2 | [🤖 Orchestration & GitOps](#gitops) | Cluster automation, ArgoCD, policy as code | `ArgoCD` `Kyverno` `HPA` |
| 3 | [🔁 Secure CI/CD Pipeline](#cicd) | Keyless, scanned, auto-rollback deploys | `Jenkins` `GH Actions` `OIDC` `Trivy` |
| 4 | [🎟️ Ticket Booking DevSecOps](#ticketbooking) | App delivered to EKS via Jenkins | `SonarQube` `OWASP` `Docker` `EKS` |
| 5 | [🎙️ VoiceOps Assistant](#voiceops) | Real-time AI voice DevOps helper | `FastAPI` `WebSocket` `Gemini Live` |

---

<a id="big-picture"></a>
## 🗺️ 0. The big picture

Five repos, one story: **build the platform → run it declaratively → ship code to it safely → get help operating it.**

```mermaid
flowchart LR
    subgraph FOUND["🏗️ Foundation"]
        P["☁️ Enterprise Cloud Platform<br/>Terraform · VPC · EKS · ALB · ASG"]
    end

    subgraph OPS["⚙️ Platform operations"]
        O["🤖 Orchestration Tool<br/>ArgoCD · Kyverno · Autoscaling"]
    end

    subgraph DELIVER["🚀 Delivery"]
        C["🔁 Secure CI/CD Pipeline<br/>Jenkins · GitHub Actions · OIDC"]
        T["🎟️ Ticket Booking<br/>Jenkins · SonarQube · Docker"]
    end

    subgraph AI["🎙️ AI assistant"]
        V["VoiceOps Assistant<br/>FastAPI · Gemini Live"]
    end

    P -->|"provides EKS cluster + VPC"| C
    O -.->|"same GitOps and policy ideas"| P
    C -->|"deploys Flask service"| EKS1[("☸️ EKS")]
    T -->|"deploys to sibi-eks"| EKS2[("☸️ EKS")]
    V -.->|"explains and diagnoses"| C
    V -.->|"explains and diagnoses"| T

    classDef found fill:#0f2027,stroke:#38bdf8,color:#ffffff
    classDef ops fill:#1e3a5f,stroke:#7dd3fc,color:#ffffff
    classDef del fill:#14532d,stroke:#4ade80,color:#ffffff
    classDef ai fill:#4c1d95,stroke:#c4b5fd,color:#ffffff
    class P found
    class O ops
    class C,T del
    class V ai
```

<details>
<summary><b>📌 What each layer teaches</b></summary>

| Layer | Skill demonstrated |
|---|---|
| Foundation | Infrastructure as Code, networking, remote state, HA design |
| Operations | GitOps, policy as code, autoscaling |
| Delivery | Shift-left security, keyless auth, automated rollback |
| AI assistant | Real-time streaming, safe read-only tooling |

</details>

---

<a id="platform"></a>
## ☁️ 1. Enterprise Cloud Infrastructure & Kubernetes Platform

<div align="center">
  <a href="https://github.com/cibi0zh/Enterprise-Cloud-Infrastructure-Kubernetes-Platform-AWS-">
    <img src="https://github-readme-stats.vercel.app/api/pin/?username=cibi0zh&repo=Enterprise-Cloud-Infrastructure-Kubernetes-Platform-AWS-&theme=tokyonight&hide_border=true" />
  </a>
</div>

A production-style, multi-AZ AWS environment that runs the same app **two ways**: on an EC2 Auto Scaling Group and on EKS.

### 🌐 Network layout

```mermaid
flowchart TB
    NET(("🌐 Internet")) --> IGW["Internet Gateway"]

    subgraph VPC["VPC 10.0.0.0/16 · 3 Availability Zones"]
        subgraph PUB["🟢 Public subnets"]
            ALB["⚖️ ALB<br/>80 / 443"]
            NAT["🔁 NAT Gateway"]
            BAS["🛡️ Bastion host<br/>SSH from admin CIDR · fail2ban"]
        end
        subgraph PRIV["🔒 Private subnets"]
            ASG["🖥️ ASG · EC2 app tier"]
            EKS["☸️ EKS managed node group"]
        end
    end

    IGW --> ALB
    IGW --> BAS
    ALB --> ASG
    NLB["ingress-nginx<br/>NLB-backed"] --> EKS
    BAS -. "SSH" .-> ASG
    ASG -->|"egress"| NAT
    EKS -->|"egress"| NAT
    NAT --> IGW

    classDef pub fill:#14532d,stroke:#4ade80,color:#ffffff
    classDef priv fill:#1e3a5f,stroke:#7dd3fc,color:#ffffff
    class ALB,NAT,BAS pub
    class ASG,EKS priv
```

### 🧱 Terraform layout and remote state

```mermaid
flowchart LR
    BS["🪣 backend-setup<br/>S3 bucket + DynamoDB lock<br/>run once"]
    BS --> DEV["🧪 environments/dev<br/>key: dev/terraform.tfstate<br/>single shared NAT"]
    BS --> PRD["🏭 environments/prod<br/>key: prod/terraform.tfstate<br/>HA NAT per AZ"]

    subgraph MODS["📦 Reusable modules (100% shared)"]
        direction TB
        M1["vpc"] ~~~ M2["security-groups"] ~~~ M3["ec2"] ~~~ M4["alb"]
        M5["asg"] ~~~ M6["s3"] ~~~ M7["iam"] ~~~ M8["eks"]
    end

    DEV --> MODS
    PRD --> MODS
```

### 📈 Kubernetes layer and observability

```mermaid
flowchart TB
    subgraph K8S["☸️ EKS cluster"]
        CHART["📦 One Helm chart: app-chart"]
        CHART -->|"values-frontend.yaml"| FE["frontend<br/>Deployment · Service · HPA · PDB"]
        CHART -->|"values-backend.yaml"| BE["backend<br/>Deployment · Service · HPA · PDB"]
        ING["ingress-nginx<br/>routes by hostname"] --> FE
        ING --> BE
        CA["Cluster Autoscaler<br/>scales node group"]
        IRSA["IRSA<br/>least-privilege AWS access"]
    end

    subgraph OBS["📊 Observability"]
        PROM["Prometheus<br/>kube-prometheus-stack"] --> GRAF["Grafana dashboards"]
        PROM --> AM["Alertmanager"] --> SLACK["💬 Slack"]
        CW["CloudWatch alarms<br/>ASG CPU · ALB 5xx · latency · unhealthy hosts"]
    end

    K8S -.->|"metrics"| PROM
    K8S -.->|"Container Insights"| CW

    classDef obs fill:#7c2d12,stroke:#fdba74,color:#ffffff
    class PROM,GRAF,AM,SLACK,CW obs
```

**Highlights**
- 🔐 Defense in depth on SSH: security groups + fail2ban + a single bastion
- 🛡️ IMDSv2 enforced, S3 encrypted and private, IRSA instead of broad node permissions
- 🐍 Boto3 and Bash automation with Cron, cutting environment setup to about 15 minutes
- ✅ CI runs `terraform validate`, tfsec and `helm lint`

**Repo:** [Enterprise-Cloud-Infrastructure-Kubernetes-Platform-AWS-](https://github.com/cibi0zh/Enterprise-Cloud-Infrastructure-Kubernetes-Platform-AWS-)

---

<a id="gitops"></a>
## 🤖 2. Orchestration Tool: Kubernetes Automation & GitOps

<div align="center">
  <a href="https://github.com/cibi0zh/orchestration-tool-Automation">
    <img src="https://github-readme-stats.vercel.app/api/pin/?username=cibi0zh&repo=orchestration-tool-Automation&theme=tokyonight&hide_border=true" />
  </a>
</div>

Provisions an EKS cluster with Terraform, delivers workloads with **Helm + ArgoCD**, and enforces policy with **Kyverno**.

```mermaid
flowchart TB
    TF["🧱 Terraform<br/>VPC + EKS module"] --> EKS["☸️ AWS EKS<br/>orchestration-tool-cluster<br/>ap-south-1"]
    BOOT["🚀 scripts/bootstrap.sh"] -->|"installs"| ARGO
    BOOT -->|"installs"| KYV
    BOOT -->|"installs"| CA

    GIT[("🐙 Git repo<br/>helm/sample-app")] -->|"watches"| ARGO["🔄 ArgoCD<br/>AppProject + Application"]
    ARGO -->|"auto-sync"| REL["📦 Helm release<br/>sample-app"]

    KYV["🛡️ Kyverno policies"] -. "admission control" .-> REL
    KYV --- P1["require resource limits"]
    KYV --- P2["disallow latest tag"]
    KYV --- P3["require non-root"]

    REL --> PODS["🟢 Pods"]
    HPA["📈 HPA"] -->|"scales pods"| PODS
    ING["🌐 NGINX Ingress + TLS"] --> PODS
    NP["🚧 NetworkPolicy"] -. "restricts traffic" .-> PODS
    CA["📏 Cluster Autoscaler"] -->|"adds nodes"| EKS
    PODS --- EKS

    classDef pol fill:#7c2d12,stroke:#fdba74,color:#ffffff
    class KYV,P1,P2,P3,NP pol
```

**Highlights**
- 🔄 Git is the source of truth: ArgoCD reconciles the cluster to match the repo
- 🛡️ Bad deployments are rejected at admission time, before they run
- 📈 Two layers of autoscaling: pods (HPA) and nodes (Cluster Autoscaler)

**Repo:** [orchestration-tool-Automation](https://github.com/cibi0zh/orchestration-tool-Automation)

---

<a id="cicd"></a>
## 🔁 3. Secure CI/CD & GitOps Automation Pipeline

<div align="center">
  <a href="https://github.com/cibi0zh/CI-CD-PIPELINE-WITH-FAILURE-ALERTS">
    <img src="https://github-readme-stats.vercel.app/api/pin/?username=cibi0zh&repo=CI-CD-PIPELINE-WITH-FAILURE-ALERTS&theme=tokyonight&hide_border=true" />
  </a>
</div>

Jenkins and GitHub Actions run the **same five stages** to ship a Flask health-check service to EKS, with no long-lived AWS keys.

```mermaid
flowchart LR
    PUSH["👨‍💻 Push / PR"] --> BUILD["🔨 Build"]
    BUILD --> TEST["🧪 Test<br/>Pytest"]
    TEST --> SCAN["🛡️ Scan<br/>Trivy · tfsec · CodeQL"]
    SCAN --> IMG["🐳 Containerize<br/>Docker → ECR<br/>scan on push"]
    IMG --> DEVD["🚀 Deploy to dev<br/>EKS"]
    DEVD --> APPR{"✋ Manual approval"}
    APPR --> PRODD["🏭 Deploy to prod"]
    PRODD --> HC{"❤️ Post-deploy<br/>health check"}
    HC -->|"pass"| LIVE["✅ Live"]
    HC -->|"fail"| RB["♻️ rollback.sh"]
    RB --> SLK["💬 Slack alert"]

    classDef ok fill:#14532d,stroke:#4ade80,color:#ffffff
    classDef bad fill:#7f1d1d,stroke:#fca5a5,color:#ffffff
    class LIVE ok
    class RB,SLK bad
```

### 🔑 Keyless AWS authentication (OIDC)

```mermaid
sequenceDiagram
    autonumber
    participant CI as GitHub Actions / Jenkins
    participant IdP as OIDC identity provider
    participant STS as AWS STS
    participant AWS as ECR + EKS

    CI->>IdP: Request short-lived identity token
    IdP-->>CI: Signed OIDC token
    CI->>STS: AssumeRoleWithWebIdentity
    STS-->>CI: Temporary credentials
    CI->>AWS: Push image and deploy
    Note over CI,AWS: No static access keys to leak or rotate
```

**Highlights**
- 🚫 Trivy and tfsec run as **required checks** that block merges on critical findings
- 🧱 Terraform (`oidc/`, `ecr/`) creates the roles and the registry
- 🧭 Separate workflows: `ci-cd.yml`, `terraform-plan.yml`, `codeql.yml`

**Repo:** [CI-CD-PIPELINE-WITH-FAILURE-ALERTS](https://github.com/cibi0zh/CI-CD-PIPELINE-WITH-FAILURE-ALERTS) · 🌐 [Live page](https://ci-cd-pipeline-with-failure-alerts.vercel.app)

---

<a id="ticketbooking"></a>
## 🎟️ 4. Ticket Booking (BookMyShow clone): DevSecOps on EKS

<div align="center">
  <a href="https://github.com/cibi0zh/TicketBooking-Application">
    <img src="https://github-readme-stats.vercel.app/api/pin/?username=cibi0zh&repo=TicketBooking-Application&theme=tokyonight&hide_border=true" />
  </a>
</div>

A Node.js movie booking app delivered by a Jenkins pipeline that scans the code, builds a Docker image, and deploys either to a container (`Jenkinsfile1`) or to **Amazon EKS** (`Jenkinsfile2`).

```mermaid
flowchart TB
    DEV["👨‍💻 Developer<br/>git push"] --> GH["🐙 GitHub<br/>TicketBooking-Application"]
    GH --> J

    subgraph J["🔧 Jenkins pipeline"]
        direction TB
        S1["🧹 Clean workspace"] --> S2["📥 Checkout"]
        S2 --> S3["🔎 SonarQube analysis"]
        S3 --> S4{"✅ Quality gate"}
        S4 --> S5["📦 npm install<br/>bookmyshow-app"]
        S5 --> S6["🛡️ OWASP Dependency-Check"]
        S6 --> S7["🛡️ Trivy filesystem scan"]
        S7 --> S8["🐳 Docker build and push"]
    end

    S8 --> DH[("🐋 Docker Hub<br/>sibi/bms:latest")]
    DH --> D1["Jenkinsfile1<br/>docker run on port 3000"]
    DH --> D2["Jenkinsfile2<br/>kubectl apply<br/>deployment.yml + service.yml"]
    D2 --> EKS["☸️ Amazon EKS<br/>sibi-eks · ap-south-1"]
    EKS --> USERS["🎬 Users book tickets"]
    D1 --> USERS
    J -. "build log + Trivy report" .-> MAIL["📧 Email notification"]
```

| File | Role |
|---|---|
| `bookmyshow-app/` | Node.js app and its Dockerfile |
| `Jenkinsfile1` | Pipeline without Kubernetes (Docker container deploy) |
| `Jenkinsfile2` | Pipeline with the EKS deploy stage |
| `deployment.yml` · `service.yml` | Kubernetes manifests |
| `BMS-Document.txt` | EKS cluster and node group notes |

**Repo:** [TicketBooking-Application](https://github.com/cibi0zh/TicketBooking-Application)

---

<a id="voiceops"></a>
## 🎙️ 5. Sibi VoiceOps Assistant: real-time AI voice for DevOps

<div align="center">
  <a href="https://github.com/cibi0zh/AI-voice-capability-check">
    <img src="https://github-readme-stats.vercel.app/api/pin/?username=cibi0zh&repo=AI-voice-capability-check&theme=tokyonight&hide_border=true" />
  </a>
</div>

The browser streams microphone audio over a WebSocket to a **FastAPI** backend, which keeps a **Gemini Live** session open and streams the voice reply back. It can only run a small set of **read-only** diagnostics.

```mermaid
sequenceDiagram
    autonumber
    actor U as 🧑 User
    participant B as 🌐 Browser UI
    participant F as ⚡ FastAPI + Uvicorn
    participant G as 🧠 Gemini Live API
    participant T as 🔍 Read-only tools

    U->>B: Speak into microphone
    B->>F: WebSocket · 16 kHz PCM audio
    F->>G: Stream audio via Google GenAI SDK
    G-->>F: Tool call request
    F->>T: CPU, memory, disk, TCP port, HTTP check, Docker list
    T-->>F: Diagnostic result
    F->>G: Return tool result
    G-->>F: Streamed voice reply
    F-->>B: Audio + transcript + tool activity
    B-->>U: Plays reply through speaker
```

```mermaid
flowchart LR
    subgraph RUN["▶️ Ways to run it"]
        A["uv run uvicorn<br/>native"] --> APP
        B2["python venv + pip"] --> APP
        C["docker compose up<br/>with Docker socket"] --> APP
        D["compose.safe.yaml<br/>no Docker socket"] --> APP
    end
    APP["🎙️ VoiceOps on port 8000<br/>/api/health"]
```

**Highlights**
- 🔒 No arbitrary shell execution and no destructive actions, by design
- 🐳 `compose.safe.yaml` runs without mounting the Docker socket
- 📝 UI shows live transcripts and which tool ran

**Repo:** [AI-voice-capability-check](https://github.com/cibi0zh/AI-voice-capability-check)

---

## 🧰 Technology matrix

| Technology | ☁️ Platform | 🤖 GitOps | 🔁 CI/CD | 🎟️ Ticket | 🎙️ Voice |
|---|:-:|:-:|:-:|:-:|:-:|
| Terraform | ✅ | ✅ | ✅ | | |
| AWS EKS | ✅ | ✅ | ✅ | ✅ | |
| Helm | ✅ | ✅ | | | |
| ArgoCD / Kyverno | | ✅ | | | |
| Jenkins | | | ✅ | ✅ | |
| GitHub Actions | ✅ | | ✅ | | |
| Trivy / tfsec | ✅ | | ✅ | ✅ | |
| SonarQube / OWASP | | | | ✅ | |
| Prometheus / Grafana | ✅ | | | | |
| Docker | | | ✅ | ✅ | ✅ |
| Python | ✅ | | ✅ | | ✅ |
| Node.js | | | | ✅ | |

---

<div align="center">

**⭐ If this helped you, star the repos, and feel free to connect on [LinkedIn](https://www.linkedin.com/in/sibi-karthik-219b1837a/)**

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:2c5364,100:0f2027&height=110&section=footer" width="100%" />

</div>
