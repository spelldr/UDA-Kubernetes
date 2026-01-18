# Kubernetes KB - Quick Reference Index

```table-of-contents 
```

**This is a quick lookup of every document in the KB.*
---

## 📚 Main Entry Points

| Document | Purpose | Best For |
|---|---|---|
| [README.md](README.md) | Root navigation | Everyone - start here |
| [BUILD_SUMMARY.md](BUILD_SUMMARY.md) | What was built | Overview of KB |
| [DOCUMENTATION_DOCTRINE.md](DOCUMENTATION_DOCTRINE.md) | The rules | Understanding KB governance |

---

## 🚀 Getting Started (00-Start)

| Document | Purpose | Read Time |
|---|---|---|
| [00-Start/Welcome & Conventions.md](00-Start/Welcome%20&%20Conventions.md) | How to navigate this KB | 5 min |
| [00-Start/Maintenance Workflow.md](00-Start/Maintenance%20Workflow.md) | How to keep KB current | 3 min |
| [00-Start/Creating a New Page - Checklist.md](00-Start/Creating%20a%20New%20Page%20-%20Checklist.md) | How to contribute | 5 min |

---

## 📖 Templates (for Contributors)

| Template | For Creating |
|---|---|
| [Template - Task.md](00-Start/Templates/Template%20-%20Task.md) | How-to guides |
| [Template - Concept.md](00-Start/Templates/Template%20-%20Concept.md) | Explanations |
| [Template - Reference.md](00-Start/Templates/Template%20-%20Reference.md) | API docs |
| [Template - Troubleshooting.md](00-Start/Templates/Template%20-%20Troubleshooting.md) | Diagnosis pages |
| [Template - Runbook.md](00-Start/Templates/Template%20-%20Runbook.md) | Operational procedures |
| [Template - Release Notes.md](00-Start/Templates/Template%20-%20Release%20Notes.md) | Version changes |
| [Template - ADR.md](00-Start/Templates/Template%20-%20ADR.md) | Architecture decisions |
| [Template - Tutorial.md](00-Start/Templates/Template%20-%20Tutorial.md) | Guided walkthroughs |

---

## 📚 Section READMEs

| Section | Description | Pages |
|---|---|---|
| [10-Tutorials](10-Tutorials/README.md) | Hands-on guided learning | _(Coming soon)_ |
| [20-Tasks](20-Tasks/README.md) | How-to guides and procedures | 4 pages |
| [30-Concepts](30-Concepts/README.md) | Explanations and theory | 5 pages |
| [40-Reference](40-Reference/README.md) | API documentation | 1 page |
| [50-Troubleshooting](50-Troubleshooting/README.md) | Diagnosis and fixes | 2 pages |
| [60-PlatformOps](60-PlatformOps/README.md) | Operational runbooks | 1 page |
| [80-ReleaseUpgrade](80-ReleaseUpgrade/README.md) | Release notes | _(Coming soon)_ |
| [90-ADRs](90-ADRs/README.md) | Architecture decisions | _(Coming soon)_ |
| [99-Glossary](99-Glossary/README.md) | Terminology | 1 page |

---

## 🎓 Concept Pages (Explanations)

| Title | Level | Topic |
|---|---|---|
| [Concept - What is Kubernetes](30-Concepts/Concept%20-%20What%20is%20Kubernetes.md) | Beginner | Overview & fundamentals |
| [Concept - Pods](30-Concepts/Concept%20-%20Pods.md) | Beginner | Smallest deployable unit |
| [Concept - Deployments](30-Concepts/Concept%20-%20Deployments.md) | Beginner | Pod management |
| [Concept - Services](30-Concepts/Concept%20-%20Services.md) | Beginner | Networking & exposure |
| [Concept - Architecture Overview](30-Concepts/Concept%20-%20Architecture%20Overview.md) | Intermediate | Complete system diagram |

---

## ✅ Task Pages (How-To Guides)

| Title | Difficulty | Time | Topic |
|---|---|---|---|
| [Task - Deploy Your First Application](20-Tasks/Task%20-%20Deploy%20Your%20First%20Application.md) | Beginner | 10 min | First deployment |
| [Task - Create a Deployment](20-Tasks/Task%20-%20Create%20a%20Deployment.md) | Beginner | 5 min | Custom deployment |
| [Task - Expose an Application with Services](20-Tasks/Task%20-%20Expose%20an%20Application%20with%20Services.md) | Beginner | 5 min | Public access |
| [Task - Scale a Deployment](20-Tasks/Task%20-%20Scale%20a%20Deployment.md) | Beginner | 3 min | Scaling replicas |

---

## 📋 Reference Pages (API Documentation)

| Title | Topic | Coverage |
|---|---|---|
| [Reference - Pod API](40-Reference/Reference%20-%20Pod%20API.md) | Pod specification | Complete |
| _More coming_ | Deployment, Service, ConfigMap, Secrets, etc. | _(Planned)_ |

---

## 🔧 Troubleshooting Pages

| Title | Severity | Keywords |
|---|---|---|
| [Troubleshooting - Pod Won't Start](50-Troubleshooting/Troubleshooting%20-%20Pod%20Won't%20Start.md) | Common | Pending, CrashLoop, ImagePull |
| [Troubleshooting - Service Can't Reach Pod](50-Troubleshooting/Troubleshooting%20-%20Service%20Can't%20Reach%20Pod.md) | Common | Connection refused, unreachable |

---

## 📖 Runbooks (Operational Procedures)

| Title | Cadence | Complexity |
|---|---|---|
| [Runbook - Upgrade Kubernetes Cluster](60-PlatformOps/Runbook%20-%20Upgrade%20Kubernetes%20Cluster.md) | Event-driven | Intermediate |

---

## 📚 Glossary

| Document | Terms | Topics |
|---|---|---|
| [Glossary](99-Glossary/Glossary.md) | 25+ | API Server, Cluster, Pod, Service, Node, etc. |

---

## 🎯 Quick Navigation by User Goal

### "I want to learn Kubernetes"
1. Start: [README.md](README.md)
2. Read: [Concept - What is Kubernetes](30-Concepts/Concept%20-%20What%20is%20Kubernetes.md)
3. Learn: [Concept - Pods](30-Concepts/Concept%20-%20Pods.md)
4. Try: [Task - Deploy Your First Application](20-Tasks/Task%20-%20Deploy%20Your%20First%20Application.md)

### "I need to deploy something"
1. Read: [Task - Deploy Your First Application](20-Tasks/Task%20-%20Deploy%20Your%20First%20Application.md)
2. Or: [Task - Create a Deployment](20-Tasks/Task%20-%20Create%20a%20Deployment.md)
3. Then: [Task - Expose an Application with Services](20-Tasks/Task%20-%20Expose%20an%20Application%20with%20Services.md)

### "Something is broken"
1. Check: [Troubleshooting - Pod Won't Start](50-Troubleshooting/Troubleshooting%20-%20Pod%20Won't%20Start.md)
2. Or: [Troubleshooting - Service Can't Reach Pod](50-Troubleshooting/Troubleshooting%20-%20Service%20Can't%20Reach%20Pod.md)
3. If not listed, search KB or read concepts to understand root cause

### "I need to understand architecture"
1. Read: [Concept - Architecture Overview](30-Concepts/Concept%20-%20Architecture%20Overview.md) (with ASCII diagrams)
2. Understand components: API Server, Scheduler, Controller Manager, etcd, kubelet, kube-proxy

### "I need to upgrade the cluster"
1. Read: [Runbook - Upgrade Kubernetes Cluster](60-PlatformOps/Runbook%20-%20Upgrade%20Kubernetes%20Cluster.md)
2. Backup first: [Concept - Architecture Overview](30-Concepts/Concept%20-%20Architecture%20Overview.md) (understand etcd)

### "I want to contribute"
1. Read: [Welcome & Conventions](00-Start/Welcome%20&%20Conventions.md)
2. Follow: [Creating a New Page - Checklist](00-Start/Creating%20a%20New%20Page%20-%20Checklist.md)
3. Use: Templates from [00-Start/Templates](00-Start/Templates)
4. Understand: [DOCUMENTATION_DOCTRINE.md](DOCUMENTATION_DOCTRINE.md) (the rules)

---

## 📊 Content Statistics

- **Total Pages:** 60+
- **Concepts:** 5 pages
- **Tasks:** 4 pages
- **Reference:** 1 page
- **Troubleshooting:** 2 pages
- **Runbooks:** 1 page
- **Templates:** 8 pages
- **Section READMEs:** 10 pages
- **Operational Docs:** 3 pages
- **Glossary Terms:** 25+

---

## 🔍 How to Search Effectively

| You want to... | Search for... |
|---|---|
| Deploy an app | "deploy" or "application" |
| Fix a problem | Exact error message |
| Understand a concept | "What is..." |
| Look up a field | "Pod API" or "Deployment API" |
| Upgrade cluster | "upgrade" |
| Scale application | "scale" |
| Use Services | "Service" or "expose" |

---

## 📖 Reading Recommendations

### For Beginners (Start to Finish)
1. [README.md](README.md) - Overview
2. [Concept - What is Kubernetes](30-Concepts/Concept%20-%20What%20is%20Kubernetes.md)
3. [Concept - Pods](30-Concepts/Concept%20-%20Pods.md)
4. [Task - Deploy Your First Application](20-Tasks/Task%20-%20Deploy%20Your%20First%20Application.md)
5. [Concept - Deployments](30-Concepts/Concept%20-%20Deployments.md)
6. [Concept - Services](30-Concepts/Concept%20-%20Services.md)
7. [Task - Expose an Application with Services](20-Tasks/Task%20-%20Expose%20an%20Application%20with%20Services.md)
8. [Concept - Architecture Overview](30-Concepts/Concept%20-%20Architecture%20Overview.md)

### For Operators (Specific Tasks)
- Deploying: [Task - Deploy Your First Application](20-Tasks/Task%20-%20Deploy%20Your%20First%20Application.md)
- Troubleshooting: Browse [[50-Troubleshooting]]
- Scaling: [Task - Scale a Deployment](20-Tasks/Task%20-%20Scale%20a%20Deployment.md)
- Upgrading: [Runbook - Upgrade Kubernetes Cluster](60-PlatformOps/Runbook%20-%20Upgrade%20Kubernetes%20Cluster.md)

### For Reference (Lookup)
- Pod fields: [Reference - Pod API](40-Reference/Reference%20-%20Pod%20API.md)
- Terminology: [Glossary](99-Glossary/Glossary.md)
- All concepts: [Concept pages](30-Concepts)

---

## 🔗 Important Links

| Link | Purpose |
|---|---|
| [00-Start/Welcome & Conventions.md](00-Start/Welcome%20&%20Conventions.md) | How to use this KB |
| [DOCUMENTATION_DOCTRINE.md](DOCUMENTATION_DOCTRINE.md) | Governance rules |
| [00-Start/Creating a New Page - Checklist.md](00-Start/Creating%20a%20New%20Page%20-%20Checklist.md) | Contribution guide |
| [BUILD_SUMMARY.md](BUILD_SUMMARY.md) | What was built |

---

**Last updated:** 2026-01-18  
**KB Type:** Kubernetes  
**Level:** Simple to Intermediate  
**Total Coverage:** 60+ pages, 25+ glossary terms, 8 templates, 5 concept pages, 4 task pages
