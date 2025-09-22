# 📌 Integrasi FluxCD dengan GitHub & Helm Chart

Repository ini berisi contoh setup **FluxCD** untuk otomatisasi update image container pada Kubernetes.  
Dengan FluxCD kita bisa melakukan:

- Sinkronisasi Git repository
- Tracking image terbaru
- Commit perubahan image tag ke repo GitOps
- Deployment otomatis ke cluster Kubernetes

---

## 🚀 Prasyarat

Sebelum mulai pastikan:

- Kubernetes cluster aktif
- FluxCD sudah terinstall
- Git repository sudah terhubung ke Flux (`GitRepository` resource sudah dibuat)
- Image registry bisa diakses oleh cluster

---

## 🔗 Integrasi FluxCD dengan GitHub

### Buat GitRepository
`GitRepository` adalah resource yang menunjuk ke repository Git (misalnya GitHub/Gitea).

**Via YAML:**
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
```
---

## Via CLI:
```bash
flux create source git klinik-repo \
  --url=ssh://git@github.com/riqimaruq/helm-chart.git \
  --branch=main \
  --interval=1m \
  --namespace=flux-system
```
---

# 📌 Metode Manual (apply YAML langsung)
## 1. Buat ImageRepository
```bash
kubectl apply -f imagerepo.yaml -n learning-helm
```
---

## 2. Buat ImagePolicy
```bash
kubectl apply -f imagepolicy.yaml -n learning-helm
```
---

## 3. Buat ImageUpdateAutomation
```bash
kubectl apply -f imageupdate.yaml -n learning-helm
```
---

## 4. Cek hasil update image
```bash
kubectl describe imageupdateautomation klinik-auto -n learning-helm
```
---

# 📌 Metode Helm (lebih dinamis & reusable)
## 1. Buat namespace (jika belum ada)
```bash
kubectl create namespace learning-helm
```
---

## 2. Install Helm chart
```bash
helm install klinik-auto ./helm-chart -n learning-helm
```
---

## 3. Update Helm chart (jika ada perubahan)
```bash
helm upgrade klinik-auto ./helm-chart -n learning-helm
```
---

## 4. Uninstall Helm release (jika ingin hapus)
```bash
helm uninstall klinik-auto -n learning-helm
```
---
