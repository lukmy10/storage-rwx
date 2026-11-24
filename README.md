# storage-rwx (GitOps Shared Storage Demo)

📦 **Companion Git repository for demonstrating Git-to-PVC synchronization using Argo CD and Rook CephFS (`ReadWriteMany`).**

This repository works alongside the **# NextGen V2 DevOps Platform: ArgoCD, GitOps & CephFS (Multi-Cloud)
** to showcase how multiple Kubernetes pods can concurrently read from and write to a shared persistent volume.

---

## ⚙️ How It Works

1. **Argo CD** monitors this repository and deploys `pvcsynchronizer.yaml` into the `argocd` namespace.
2. The **`git-to-pvc-synchronizer`** Deployment runs a `git-sync` container (`fsGroup: 65533`) that pulls the `main` branch every **60 seconds**.
3. Pulled files (`test2`, `test3`, `test4`, etc.) are written directly into the shared CephFS volume (`rbd-pvc-nginx-argocd-rwx`) under `/storage-rwx` and immediately served by the live **Nginx** pod.

---

## 📂 Repository Contents

* **`pvcsynchronizer.yaml`** — Kubernetes `Deployment` manifest running `git-sync` to continuously sync this repo into the shared RWX `PersistentVolumeClaim`.
* **`test2` / `test3` / `test4`** — Sample test files used to verify live Git-to-Nginx synchronization.

---

## 🚀 Quick Usage in Argo CD

1. Open your **Argo CD Web Portal** and click **+ NEW APP**.
2. Set the **Source** to:
   * **Repository URL:** `https://github.com/lukmy10/storage-rwx`
   * **Revision:** `main`
   * **Path:** `.`
3. Set the **Destination** to:
   * **Cluster URL:** `https://default.svc`
   * **Namespace:** `argocd`
4. Click **CREATE** and sync the application.
5. Verify live updates in your browser via your Nginx endpoint:
   ```text
   http://<NODE_PUBLIC_IP>:<NGINX_NODE_PORT>/storage-rwx/test2
   ```

---

## 📄 License

MIT License
# storage
