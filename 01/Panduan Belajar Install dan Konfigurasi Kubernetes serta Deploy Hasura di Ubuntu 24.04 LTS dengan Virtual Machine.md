# Panduan Installasi Kubernetes dan Hasura Menggunakan Ubuntu

## 1. Download Ubuntu Server atau Desktop
- **Ubuntu Server 24.04 LTS**  
  [Download Link](https://ubuntu.com/download/server/thank-you?version=24.04&architecture=amd64&lts=true)

- **Ubuntu Desktop 24.04 LTS**  
  [Download Link](https://ubuntu.com/download/desktop/thank-you?version=24.04.1&architecture=amd64&lts=true)

## 2. Install VirtualBox
Untuk menjalankan Virtual Machine, unduh dan install VirtualBox:  
[VirtualBox Download](https://www.virtualbox.org/wiki/Downloads)

## 3. Konfigurasi Virtual Machine
Buat dua Virtual Machine (VM) dengan spesifikasi berikut:
- **RAM**: Minimal 2 GB (disarankan 4 GB atau lebih)
- **Storage**: Minimal 20 GB
- **Processors**: 2 CPU
- **Network Adapter**: Bridged Adapter
- **Static IP Configuration**:  
  [Panduan Konfigurasi Static IP](https://linuxconfig.org/setting-a-static-ip-address-in-ubuntu-24-04-via-the-command-line)  
  [Video Tutorial](https://drive.google.com/file/d/1WiwVkzrj4NWQRhGHzuArSKpplOYetDhS/view?usp=sharing)

## 4. Install Docker, Kubernetes, dan Setup Cluster
Buat VM pertama sebagai **node master** dan VM kedua sebagai **node worker**.  
Untuk menginstall Docker, Kubernetes, dan melakukan konfigurasi cluster, ikuti panduan berikut:  
[Panduan Setup Kubernetes Cluster](https://www.redswitches.com/blog/install-kubernetes-cluster-ubuntu/)

## 5. Pemahaman Dasar Kubernetes
Untuk mempelajari lebih dalam tentang Kubernetes, video berikut bisa sangat membantu:  
[Video Penjelasan Kubernetes](https://youtu.be/96mqy5iCjoA?si=JSVp1Ts2Pfm16PEE)

## 6. Install Kubernetes Dashboard
Untuk mempermudah monitoring Kubernetes, install Kubernetes Dashboard:  
[Video Tutorial Install Dashboard](https://youtu.be/8J2lN2EJ_Jo?si=v2VqjnePGPvY82xv)

## 7. Deploy Hasura di Kubernetes Menggunakan Helm
Untuk deploy Hasura di atas Kubernetes, ikuti panduan berikut:  
[Hasura Helm Deployment Guide](https://hasura.io/docs/2.0/deployment/deployment-guides/kubernetes-helm/)  
Sebagai tambahan config.yaml untuk pv Hasura adalah sebagai berikut:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: hasura-postgres-pv
  labels:
    app: hasura-postgres
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  volumeMode: Filesystem
  storageClassName: ""
  hostPath:
    path: "/mnt/data"

