# Humanitec Score + Backstage CloudNativePG DBaaS Demo

This project provides a **PostgreSQL as a Service (DBaaS)** experience for developers using:

- [CloudNativePG](https://cloudnative-pg.io) as the operator-powered Postgres engine
- [Score](https://score.dev) to define portable database consumers
- [Humanitec Resource Definitions](https://docs.humanitec.com/integrations/resource-definitions) for dynamic provisioning
- [Backstage](https://backstage.io) for developer self-service
- [PocketIDP](https://github.com/Humanitec-Dev/PocketIDP) to emulate the full platform locally

> 🧠 Platform Engineers can transform complex database provisioning into a one-click, self-service experience — integrated into GitOps workflows and golden paths.

---

## 🧰 Prerequisites

Ensure you have the following:

| Tool                         | Description                                                  |
|------------------------------|--------------------------------------------------------------|
| ✅ [PocketIDP]               | Runs Backstage, GitOps, and Humanitec Agent locally          |
| ✅ Humanitec Account         | Required to apply Resource Definitions & deploy workloads     |
| ✅ `score` CLI               | To define and validate app workloads                          |
| ✅ `humctl` CLI              | To interact with the Humanitec Platform via CLI               |
| ✅ `kubectl` + Cluster       | Needed to install CloudNativePG locally (e.g., with kind/minikube)

---

## 👷 Platform Engineer Workflow

Platform Engineers enable database provisioning via reusable golden paths and dynamic resource mappings.

### 🔧 Step-by-Step:

1. **Install CloudNativePG Operator**

   On your local Kubernetes cluster (kind, minikube, etc.):

   ```bash
   kubectl apply -f https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/main/releases/cnpg-1.22.0.yaml

2. **Apply Resource Definitions**

   Define a custom resource that provisions CloudNativePG clusters and Postgres credentials. These are located in:

   ```
   ./humanitec-resource-definitions/
   ```

   Apply via:

   ```bash
   humctl apply -f humanitec-resource-definitions/
   ```

3. **Register Backstage Templates**

   Register DBaaS templates under `./backstage-templates/` using either:

   * Static `catalog-info.yaml`
   * Git sync via PocketIDP's Gitea
   * via Backstage UI

4. **Start PocketIDP**

   Spin up a full local IDP stack: refer to PocketIDP repo

   ---

## 👨‍💻 Developer Workflow

After platform engineers configure everything:

1. **Access Backstage**

   Visit the Backstage portal at [http://localhost:7007](http://localhost:7007)

2. **Choose a Postgres Template**

   Use a Golden Path template like **“Provision PostgreSQL DBaaS”**

3. **Fill Out Parameters**

   Set environment, DB name, user, and namespace.

4. **Backstage Pushes Workload to Git**

   A scaffolded Score workload with DB dependency is pushed to Gitea (or GitHub).

5. **Humanitec Provisions Postgres**

   The platform uses your Score spec + DBaaS resource definition to:

   * Spin up a CloudNativePG cluster (via CRDs)
   * Inject Postgres credentials into the consuming service
   * Bind it into the workload

---

## ⚙️ Local Deployment with Score Drivers

> 🧱 This demo builds on the official [**Score + CloudNativePG DBaaS tutorials**](https://github.com/InternalDeveloperPlatform/PocketIDP/Tutorials) under the [Humanitec-PocketIDP](https://github.com/InternalDeveloperPlatform) umbrella.

If you want to run the workload locally via CLI:

```bash
# Validate the workload spec
score validate -f score.yaml

# Option 1: Local deployment via score-compose
score-compose run -f score.yaml

# Option 2: Kubernetes deployment via score-k8s
score-k8s run -f score.yaml

# Option 3: Deploy via Humanitec API
humctl deploy \
  --app cloudnativepg-dbaas-demo \
  --env development \
  -f score.yaml \
  --message "Provisioning DB with Humanitec"
```

✅ The official demo focuses on **CLI-based deployments**
✅ This project extends it into a **fully self-serviced, visual IDP flow using Backstage and PocketIDP**

---

## 📁 Project Structure

```
.
├── score.yaml                         # Postgres consumer workload
├── backstage-templates/              # Backstage software templates (DBaaS and sample app)
├── humanitec-resource-definitions/   # CloudNativePG-backed DB resource logic
├── .github/workflows/ci.yaml         # Optional CI pipeline
└── README.md                         # This file
```

---

## 🧠 Key Concepts

* **CloudNativePG** operator for true cloud-native Postgres
* **Score** as the abstraction layer between app and infra
* **Humanitec Resource Definitions** to dynamically provision DBs
* **Backstage** for Golden Path and UI-driven consumption
* **PocketIDP** to simulate the full experience locally

---

## 📘 Related Repos

* 🔗 [CloudNativePG GitHub](https://github.com/cloudnative-pg/cloudnative-pg)
* 🔗 [Humanitec-DemoOrg DBaaS Examples](https://github.com/Humanitec-DemoOrg)
* 🔗 [Score Examples](https://github.com/score-spec/examples)

---

## 🛡 Maintainers

This demo is maintained by Platform Engineers contributing to Humanitec Demo Org — focused on turning CLI demos into Golden Paths and self-service IDP flows.

---

## 📚 Resources

* [Score.dev](https://score.dev)
* [Humanitec Docs](https://docs.humanitec.com)
* [CloudNativePG](https://cloudnative-pg.io)
* [PocketIDP](https://github.com/Humanitec-Dev/PocketIDP)
* [Backstage](https://backstage.io)
* [Platform Engineering Slack](https://platformengineering.org/slack)

```
