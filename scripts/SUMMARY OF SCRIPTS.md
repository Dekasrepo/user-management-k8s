## 📝 Summary of Scripts

### 1. scripts/setup-tls.sh - TLS Certificate Generator
Purpose: Automates the entire process of creating TLS certificates for HTTPS
What it does:

- ✅ Checks if mkcert is installed
- ✅ Installs mkcert Certificate Authority (CA) if needed
- ✅ Generates TLS certificates for your domain (default: jideka.com.ng)
- ✅ Base64 encodes the certificate and private key
- ✅ Creates the Kubernetes TLS Secret YAML file (k8s/04-tls-secret.yaml)
- ✅ Verifies the generated files
- ✅ Shows you the next steps

**Usage:**

- Make executable
```
chmod +x scripts/setup-tls.sh
```

- Generate certificates for default domain (jideka.com.ng)
```
./scripts/setup-tls.sh
```

- Generate for custom domain
```
./scripts/setup-tls.sh mydomain.com
```

Output:
- tls/jideka.com.ng.pem - Certificate
- tls/jideka.com.ng-key.pem - Private key
- tls/tls.crt.base64 - Base64 encoded certificate
- tls/tls.key.base64 - Base64 encoded key
- k8s/04-tls-secret.yaml - Kubernetes secret


### 2. scripts/deploy.sh - Complete Deployment Script
Purpose: Deploys the entire application to Kubernetes in the correct order
What it does:

- ✅ Checks prerequisites (kubectl, minikube)
- ✅ Starts Minikube if not running
- ✅ Enables ingress addon
- ✅ Deploys all Kubernetes manifests in order:
  - Namespace
  - ConfigMap
  - Secret
  - TLS Secret
  - MongoDB (PVC, Deployment, Service)
  - Backend (Deployment, Service)
  - Frontend (Deployment, Service)
  - Ingress
- ✅ Waits for pods to be ready
- ✅ Shows deployment summary
- ✅ Tests backend health
- ✅ Displays access instructions

**Usage:**
- Make executable
```
chmod +x scripts/deploy.sh
```

- Deploy everything
```
./scripts/deploy.sh
```

 - Deploy and watch pods
```
./scripts/deploy.sh --watch
```

- Skip Minikube checks (for cloud deployments)
```
./scripts/deploy.sh --skip-minikube
```

Options:
- -s, --skip-minikube - Skip Minikube checks
- -w, --watch - Watch pod status after deployment
- -h, --help - Show help

### 3. scripts/cleanup.sh - Cleanup Script
Purpose: Safely removes all deployed resources
What it does:

- ✅ Shows what will be deleted
- ✅ Asks for confirmation (unless forced)
- ✅ Deletes resources based on options:
  - All resources in namespace
  - Entire namespace
  - Persistent volumes (database data)
  - Minikube cluster
- ✅ Cleans up local TLS files
- ✅ Shows cleanup summary

**Usage:**

- Make executable
```
chmod +x scripts/cleanup.sh
```

- Remove all resources (keeps namespace)

```
./scripts/cleanup.sh
```

- Remove resources and database data
```
./scripts/cleanup.sh --volumes
```

- Remove everything including Minikube
```
./scripts/cleanup.sh --all
```

- Force cleanup without prompts
```
./scripts/cleanup.sh --force --volumes
```

- Keep namespace, delete only resources
```
./scripts/cleanup.sh --keep-namespace
```

Options:
- -a, --all - Remove everything including Minikube
- -v, --volumes - Remove volumes (⚠️ deletes database data!)
- -k, --keep-namespace - Keep namespace, only delete resources
- -f, --force - Skip confirmation prompts
- -h, --help - Show help

⚠️ Warning: Using --volumes will permanently delete all database data!


### 🚀 Quick Start Workflow
Here's how to use all three scripts together:

**1. Generate TLS certificates**
```
./scripts/setup-tls.sh
```

**2. Deploy everything**
```
./scripts/deploy.sh --watch
```

**3. Access your app**

Open: https://jideka.com.ng

**4. When done, cleanup**
```
./scripts/cleanup.sh
```

**5. For complete cleanup (removes everything)**
```
./scripts/cleanup.sh --all --volumes --force

```
