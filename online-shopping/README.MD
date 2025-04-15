# **Kustomize: Kubernetes Native Configuration Management**

Kustomize is a configuration management tool specifically designed for Kubernetes. It allows you to customize and manage Kubernetes manifests without needing to modify the original YAML files directly. Kustomize enables a clean and reusable approach to managing Kubernetes deployments across multiple environments, such as development, staging, and production.

---

## 📚 **Table of Contents**

1. [What is Kustomize?](#what-is-kustomize)
2. [Why Use Kustomize?](#why-use-kustomize)
3. [Key Concepts](#key-concepts)
4. [Installation](#installation)
5. [Getting Started](#getting-started)
6. [Directory Structure](#directory-structure)
7. [Basic Example](#basic-example)
8. [Commands](#commands)
9. [Best Practices](#best-practices)
10. [Resources](#resources)

---

## 🚀 **What is Kustomize?**

Kustomize is a tool for customizing Kubernetes YAML configurations. It allows you to define, reuse, and compose Kubernetes resources while keeping your configurations clean and maintainable.

**Core Features:**
- Declarative YAML-based configurations.
- Layered customization for different environments.
- Patching existing resources without duplication.
- Built-in support for secret and config map generation.

---

## 🤔 **Why Use Kustomize?**

- **Environment-Specific Configurations:** Manage configurations for development, staging, and production without duplicating YAML files.
- **No Templates:** Avoid template engines (e.g., Helm). Instead, use straightforward YAML patches.
- **Reusability:** Reuse Kubernetes manifests across different environments.
- **Integration:** Natively integrated into `kubectl`.

---

## 📌 **Key Concepts**

- **Base:** Common Kubernetes manifests shared across all environments.
- **Overlay:** Environment-specific customizations applied on top of the base.
- **kustomization.yaml:** The configuration file that defines resources, patches, and customizations.
- **Resources:** Kubernetes manifests referenced by the `kustomization.yaml` file.
- **Patches:** YAML snippets used to modify existing resources.

---

## 🛠️ **Installation**

Kustomize comes bundled with `kubectl` (v1.14+). To verify:

```bash
kubectl kustomize --help
```

Alternatively, install it as a standalone tool:

```bash
# On macOS
brew install kustomize

# On Linux
curl -Lo kustomize https://github.com/kubernetes-sigs/kustomize/releases/latest/download/kustomize_linux_amd64
chmod +x kustomize
sudo mv kustomize /usr/local/bin/
```

Verify installation:

```bash
kustomize version
```

---

## 🏁 **Getting Started**

### 1. **Create a Base Directory**
```bash
mkdir -p kustomize/base
cd kustomize/base
```

### 2. **Add Kubernetes Manifests**
Add your Kubernetes resource files (`deployment.yaml`, `service.yaml`, etc.).

### 3. **Create a kustomization.yaml**
```yaml
resources:
  - deployment.yaml
  - service.yaml
```

### 4. **Create Overlays**
```bash
mkdir -p ../overlays/dev ../overlays/prod
```

Add environment-specific patches in each overlay.

### 5. **Build Configuration**
```bash
kustomize build overlays/dev
```

Apply to your cluster:
```bash
kubectl apply -k overlays/dev
```

---

## 📁 **Directory Structure**

A typical Kustomize directory structure looks like this:

```
kustomize/
├── base/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── kustomization.yaml
├── overlays/
│   ├── dev/
│   │   ├── kustomization.yaml
│   │   ├── deployment-patch.yaml
│   ├── prod/
│       ├── kustomization.yaml
│       ├── deployment-patch.yaml
```

---

## ⚙️ **Basic Example**

### **Base Configuration (`base/kustomization.yaml`)**
```yaml
resources:
  - deployment.yaml
  - service.yaml
```

### **Dev Overlay (`overlays/dev/kustomization.yaml`)**
```yaml
resources:
  - ../../base

patches:
  - path: deployment-patch.yaml
```

### **Deployment Patch (`overlays/dev/deployment-patch.yaml`)**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
```

### **Build and Apply**
```bash
kustomize build overlays/dev | kubectl apply -f -
```

---

## 📊 **Commands**

- **Build:** Renders YAML configurations based on `kustomization.yaml`.
  ```bash
  kustomize build ./overlays/dev
  ```

- **Apply:** Apply configurations to Kubernetes.
  ```bash
  kubectl apply -k ./overlays/dev
  ```

- **View Diff:** Show differences before applying changes.
  ```bash
  kubectl diff -k ./overlays/dev
  ```

---

## 🧠 **Best Practices**

- Use **base** for shared resources and **overlays** for environment-specific patches.
- Avoid duplicating YAML files across overlays.
- Leverage **patchesStrategicMerge** for targeted changes.
- Regularly validate your configurations using `kustomize build`.

---

## 📚 **Resources**

- [Kustomize Documentation](https://kustomize.io)
- [Kubernetes Official Docs](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/)
- [Kustomize GitHub Repository](https://github.com/kubernetes-sigs/kustomize)

---

Start managing your Kubernetes resources efficiently with **Kustomize**! 🚀

