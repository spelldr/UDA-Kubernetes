# Glossary

Common Kubernetes terms and definitions.

## How to Use This Glossary

Search (Ctrl+Shift+F) for a term you don't understand. Each entry is concise and includes links to related pages.

---

## Terms

### **API Server**
The component that serves the Kubernetes REST API. All operations in Kubernetes go through the API server. See [[Concept - Architecture Overview]].

### **Cluster**
A set of nodes running Kubernetes, managed by a control plane. The unit of deployment in Kubernetes. See [[Concept - What is Kubernetes]].

### **Container**
A standardized package containing code, runtime, system tools, and libraries. Kubernetes doesn't run containers directly—it runs [[#Pod | Pods]].

### **Control Plane**
The Kubernetes brain: API server, scheduler, controller manager, and etcd. Manages the cluster state. See [[Concept - Architecture Overview]].

### **Deployment**
A Kubernetes resource that describes a desired state for Pods: how many replicas, which image, update strategy, etc. See [[Concept - Deployments]] and [[Task - Create a Deployment]].

### **Desired State**
The configuration you declare: "I want 3 replicas of nginx:1.24 running." Kubernetes reconciles the actual state to match desired state.

### **etcd**
A distributed key-value store. Kubernetes' database that stores all cluster state. See [[Concept - Architecture Overview]].

### **Image**
A snapshot of a containerized application. Includes code, runtime, dependencies. Referenced in [[#Pod | Pods]] as `image: nginx:1.24`.

### **kubectl**
The command-line tool for interacting with Kubernetes. Used to create, read, update, and delete resources.

### **Manifest**
A YAML or JSON file describing Kubernetes resources. Example: a Pod manifest, a Deployment manifest. Usually applied with `kubectl apply -f manifest.yaml`.

### **Namespace**
A logical partition within a cluster. Multiple namespaces can coexist in one cluster. Default namespace is `default`. See [[Concept - Namespaces]].

### **Node**
A machine (physical or virtual) in a Kubernetes cluster. Runs Pods. Managed by the control plane. See [[Concept - Nodes]].

### **Pod**
The smallest deployable unit in Kubernetes. Usually wraps one container, but can wrap multiple containers. Containers in a Pod share networking. See [[Concept - Pods]].

### **Reconciliation**
Kubernetes' continuous loop: "Is the actual state matching the desired state? If not, fix it." Powers self-healing.

### **Replica**
A copy. If you want 3 replicas of an app, you want 3 identical Pods running. See [[Concept - Deployments]].

### **Replica Set**
A lower-level Kubernetes resource that ensures a specified number of identical Pods are running. Usually managed by a [[#Deployment | Deployment]].

### **Service**
A stable way to expose Pods. Provides DNS name and load balancing across multiple Pods. See [[Concept - Services]] and [[Task - Expose an Application with Services]].

### **StatefulSet**
Like a Deployment, but for stateful applications (databases, caches). Guarantees stable Pod identities and persistent storage. See [[Concept - StatefulSets]].

### **Volume**
Storage that's mounted into a Pod. Can be ephemeral (deleted with Pod) or persistent (survives Pod recreation).

---

## How to Contribute

Found a term not in the glossary? 
1. Check [[00-Start/Creating a New Page - Checklist]]
2. Add the term here in alphabetical order
3. Include a brief definition and relevant links
