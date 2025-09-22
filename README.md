## 📌 Integrasi dengan FluxCD

FluxCD memungkinkan otomatisasi penuh mulai dari **sinkronisasi Git repository**, **tracking image baru**, hingga **commit perubahan image tag** ke repo GitOps.
### Integrasi flux-cd dengan github
flux create source git klinik-repo   --url=ssh://git@github.com/riqimaruq/helm-chart.git   --branch=main   --interval=1m   --namespace=flux-system
### 1. Buat `GitRepository`
`GitRepository` adalah resource yang menunjuk ke repository Git kamu (misalnya GitHub/Gitea).

```yaml
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: klinik-repo
  namespace: flux-system
spec:
  interval: 1m
  url: ssh://git@github.com/riqimaruq/klinik-gitops.git
  ref:
    branch: main
  secretRef:
    name: flux-system

# FluxCD Image Automation

Repository ini berisi contoh setup **FluxCD** untuk melakukan otomatisasi update image container pada Kubernetes.  
Terdapat dua metode yang bisa digunakan:

1. **Metode Manual (apply YAML langsung)**
2. **Metode Helm (lebih dinamis & reusable)**

---

## 🚀 Prasyarat

- Kubernetes cluster aktif
- FluxCD sudah terinstall
- Git repository sudah terhubung ke Flux (`GitRepository` resource sudah dibuat)
- Image registry sudah bisa diakses oleh cluster

---

## 📌 Metode Manual

### 1. Buat `ImageRepository`
```bash
kubectl apply -f imagerepo.yaml -n learning-helm
### 2. Buat ImagePolicy
kubectl apply -f imagepolicy.yaml -n learning-helm

### 3. Buat ImageUpdateAutomation
kubectl apply -f imageupdate.yaml -n learning-helm

### 4. Cek hasil update image
kubectl describe imageupdateautomation klinik-auto -n learning-helm

### 📌 Metode Helm
### 1. Buat namespace (jika belum ada)
kubectl create namespace learning-helm

### 2. Install Helm chart
helm install klinik-auto ./helm-chart -n learning-helm

### 3. Update Helm chart (jika ada perubahan)
helm upgrade klinik-auto ./helm-chart -n learning-helm

### 4. Uninstall Helm release (jika ingin hapus)
helm uninstall klinik-auto -n learning-helm

### 📌 Metode Helm
### 1. Buat namespace (jika belum ada)
kubectl create namespace learning-helm

### 2. Install Helm chart
helm install klinik-auto ./helm-chart -n learning-helm

### 3. Update Helm chart (jika ada perubahan)
helm upgrade klinik-auto ./helm-chart -n learning-helm

### 4. Uninstall Helm release (jika ingin hapus)
helm uninstall klinik-auto -n learning-helm
