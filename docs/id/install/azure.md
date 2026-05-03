---
source_url: https://docs.openclaw.ai/id/install/azure
title: "Azure - OpenClaw"
---

[Langsung ke konten utama](https://docs.openclaw.ai/id/install/azure#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/id)

![ID](https://d3gk2c5xim1je2.cloudfront.net/flags/ID.svg)

Bahasa Indonesia

Cari...

Ctrl K

Cari...

Navigation

Hosting

Azure

[Get started](https://docs.openclaw.ai/id) [Install](https://docs.openclaw.ai/id/install) [Channels](https://docs.openclaw.ai/id/channels) [Agents](https://docs.openclaw.ai/id/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/id/tools) [Models](https://docs.openclaw.ai/id/providers) [Platforms](https://docs.openclaw.ai/id/platforms) [Gateway & Ops](https://docs.openclaw.ai/id/gateway) [Reference](https://docs.openclaw.ai/id/cli) [Help](https://docs.openclaw.ai/id/help)

Di halaman ini

- [OpenClaw di Azure Linux VM](https://docs.openclaw.ai/id/install/azure#openclaw-di-azure-linux-vm)
- [Yang akan Anda lakukan](https://docs.openclaw.ai/id/install/azure#yang-akan-anda-lakukan)
- [Yang Anda butuhkan](https://docs.openclaw.ai/id/install/azure#yang-anda-butuhkan)
- [Konfigurasikan deployment](https://docs.openclaw.ai/id/install/azure#konfigurasikan-deployment)
- [Deploy resource Azure](https://docs.openclaw.ai/id/install/azure#deploy-resource-azure)
- [Pasang OpenClaw](https://docs.openclaw.ai/id/install/azure#pasang-openclaw)
- [Pertimbangan biaya](https://docs.openclaw.ai/id/install/azure#pertimbangan-biaya)
- [Pembersihan](https://docs.openclaw.ai/id/install/azure#pembersihan)
- [Langkah selanjutnya](https://docs.openclaw.ai/id/install/azure#langkah-selanjutnya)
- [Terkait](https://docs.openclaw.ai/id/install/azure#terkait)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/id/install/azure\#openclaw-di-azure-linux-vm)  OpenClaw di Azure Linux VM

Panduan ini menyiapkan Azure Linux VM dengan Azure CLI, menerapkan hardening Network Security Group (NSG), mengonfigurasi Azure Bastion untuk akses SSH, dan memasang OpenClaw.

## [​](https://docs.openclaw.ai/id/install/azure\#yang-akan-anda-lakukan)  Yang akan Anda lakukan

- Membuat resource jaringan Azure (VNet, subnet, NSG) dan komputasi dengan Azure CLI
- Menerapkan aturan Network Security Group sehingga SSH VM hanya diizinkan dari Azure Bastion
- Menggunakan Azure Bastion untuk akses SSH (tanpa IP publik pada VM)
- Memasang OpenClaw dengan skrip installer
- Memverifikasi Gateway

## [​](https://docs.openclaw.ai/id/install/azure\#yang-anda-butuhkan)  Yang Anda butuhkan

- Langganan Azure dengan izin untuk membuat resource komputasi dan jaringan
- Azure CLI terpasang (lihat [langkah pemasangan Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) bila diperlukan)
- Sepasang kunci SSH (panduan ini mencakup pembuatan jika diperlukan)
- ~20-30 menit

## [​](https://docs.openclaw.ai/id/install/azure\#konfigurasikan-deployment)  Konfigurasikan deployment

1

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Masuk ke Azure CLI

```
az login
az extension add -n ssh
```

Ekstensi `ssh` diperlukan untuk tunneling SSH native Azure Bastion.

2

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Daftarkan provider resource yang diperlukan (sekali saja)

```
az provider register --namespace Microsoft.Compute
az provider register --namespace Microsoft.Network
```

Verifikasi pendaftaran. Tunggu sampai keduanya menampilkan `Registered`.

```
az provider show --namespace Microsoft.Compute --query registrationState -o tsv
az provider show --namespace Microsoft.Network --query registrationState -o tsv
```

3

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Tetapkan variabel deployment

```
RG="rg-openclaw"
LOCATION="westus2"
VNET_NAME="vnet-openclaw"
VNET_PREFIX="10.40.0.0/16"
VM_SUBNET_NAME="snet-openclaw-vm"
VM_SUBNET_PREFIX="10.40.2.0/24"
BASTION_SUBNET_PREFIX="10.40.1.0/26"
NSG_NAME="nsg-openclaw-vm"
VM_NAME="vm-openclaw"
ADMIN_USERNAME="openclaw"
BASTION_NAME="bas-openclaw"
BASTION_PIP_NAME="pip-openclaw-bastion"
```

Sesuaikan nama dan rentang CIDR agar sesuai dengan environment Anda. Subnet Bastion harus minimal `/26`.

4

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Pilih kunci SSH

Gunakan kunci publik yang sudah ada jika Anda memilikinya:

```
SSH_PUB_KEY="$(cat ~/.ssh/id_ed25519.pub)"
```

Jika Anda belum memiliki kunci SSH, buat satu:

```
ssh-keygen -t ed25519 -a 100 -f ~/.ssh/id_ed25519 -C "you@example.com"
SSH_PUB_KEY="$(cat ~/.ssh/id_ed25519.pub)"
```

5

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Pilih ukuran VM dan ukuran disk OS

```
VM_SIZE="Standard_B2as_v2"
OS_DISK_SIZE_GB=64
```

Pilih ukuran VM dan ukuran disk OS yang tersedia pada langganan dan region Anda:

- Mulai dari yang lebih kecil untuk penggunaan ringan dan tingkatkan nanti
- Gunakan lebih banyak vCPU/RAM/disk untuk otomatisasi yang lebih berat, lebih banyak channel, atau beban kerja model/alat yang lebih besar
- Jika ukuran VM tidak tersedia di region atau kuota langganan Anda, pilih SKU terdekat yang tersedia

Daftar ukuran VM yang tersedia di region target Anda:

```
az vm list-skus --location "${LOCATION}" --resource-type virtualMachines -o table
```

Periksa penggunaan/kuota vCPU dan disk Anda saat ini:

```
az vm list-usage --location "${LOCATION}" -o table
```

## [​](https://docs.openclaw.ai/id/install/azure\#deploy-resource-azure)  Deploy resource Azure

1

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Buat resource group

```
az group create -n "${RG}" -l "${LOCATION}"
```

2

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Buat network security group

Buat NSG dan tambahkan aturan agar hanya subnet Bastion yang dapat SSH ke VM.

```
az network nsg create \
  -g "${RG}" -n "${NSG_NAME}" -l "${LOCATION}"

# Izinkan SSH hanya dari subnet Bastion
az network nsg rule create \
  -g "${RG}" --nsg-name "${NSG_NAME}" \
  -n AllowSshFromBastionSubnet --priority 100 \
  --access Allow --direction Inbound --protocol Tcp \
  --source-address-prefixes "${BASTION_SUBNET_PREFIX}" \
  --destination-port-ranges 22

# Tolak SSH dari internet publik
az network nsg rule create \
  -g "${RG}" --nsg-name "${NSG_NAME}" \
  -n DenyInternetSsh --priority 110 \
  --access Deny --direction Inbound --protocol Tcp \
  --source-address-prefixes Internet \
  --destination-port-ranges 22

# Tolak SSH dari sumber VNet lain
az network nsg rule create \
  -g "${RG}" --nsg-name "${NSG_NAME}" \
  -n DenyVnetSsh --priority 120 \
  --access Deny --direction Inbound --protocol Tcp \
  --source-address-prefixes VirtualNetwork \
  --destination-port-ranges 22
```

Aturan dievaluasi berdasarkan prioritas (angka terkecil lebih dulu): lalu lintas Bastion diizinkan pada 100, lalu semua SSH lain diblokir pada 110 dan 120.

3

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Buat virtual network dan subnet

Buat VNet dengan subnet VM (NSG terpasang), lalu tambahkan subnet Bastion.

```
az network vnet create \
  -g "${RG}" -n "${VNET_NAME}" -l "${LOCATION}" \
  --address-prefixes "${VNET_PREFIX}" \
  --subnet-name "${VM_SUBNET_NAME}" \
  --subnet-prefixes "${VM_SUBNET_PREFIX}"

# Pasang NSG ke subnet VM
az network vnet subnet update \
  -g "${RG}" --vnet-name "${VNET_NAME}" \
  -n "${VM_SUBNET_NAME}" --nsg "${NSG_NAME}"

# AzureBastionSubnet — nama ini diwajibkan oleh Azure
az network vnet subnet create \
  -g "${RG}" --vnet-name "${VNET_NAME}" \
  -n AzureBastionSubnet \
  --address-prefixes "${BASTION_SUBNET_PREFIX}"
```

4

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Buat VM

VM tidak memiliki IP publik. Akses SSH dilakukan secara eksklusif melalui Azure Bastion.

```
az vm create \
  -g "${RG}" -n "${VM_NAME}" -l "${LOCATION}" \
  --image "Canonical:ubuntu-24_04-lts:server:latest" \
  --size "${VM_SIZE}" \
  --os-disk-size-gb "${OS_DISK_SIZE_GB}" \
  --storage-sku StandardSSD_LRS \
  --admin-username "${ADMIN_USERNAME}" \
  --ssh-key-values "${SSH_PUB_KEY}" \
  --vnet-name "${VNET_NAME}" \
  --subnet "${VM_SUBNET_NAME}" \
  --public-ip-address "" \
  --nsg ""
```

`--public-ip-address ""` mencegah IP publik ditetapkan. `--nsg ""` melewati pembuatan NSG per-NIC (NSG tingkat subnet menangani keamanan).**Reproduksibilitas:** Perintah di atas menggunakan `latest` untuk image Ubuntu. Untuk menyematkan versi tertentu, daftar versi yang tersedia lalu ganti `latest`:

```
az vm image list \
  --publisher Canonical --offer ubuntu-24_04-lts \
  --sku server --all -o table
```

5

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Buat Azure Bastion

Azure Bastion menyediakan akses SSH terkelola ke VM tanpa mengekspos IP publik. SKU Standard dengan tunneling diperlukan untuk `az network bastion ssh` berbasis CLI.

```
az network public-ip create \
  -g "${RG}" -n "${BASTION_PIP_NAME}" -l "${LOCATION}" \
  --sku Standard --allocation-method Static

az network bastion create \
  -g "${RG}" -n "${BASTION_NAME}" -l "${LOCATION}" \
  --vnet-name "${VNET_NAME}" \
  --public-ip-address "${BASTION_PIP_NAME}" \
  --sku Standard --enable-tunneling true
```

Provisioning Bastion biasanya memakan waktu 5-10 menit tetapi bisa sampai 15-30 menit di beberapa region.

## [​](https://docs.openclaw.ai/id/install/azure\#pasang-openclaw)  Pasang OpenClaw

1

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

SSH ke VM melalui Azure Bastion

```
VM_ID="$(az vm show -g "${RG}" -n "${VM_NAME}" --query id -o tsv)"

az network bastion ssh \
  --name "${BASTION_NAME}" \
  --resource-group "${RG}" \
  --target-resource-id "${VM_ID}" \
  --auth-type ssh-key \
  --username "${ADMIN_USERNAME}" \
  --ssh-key ~/.ssh/id_ed25519
```

2

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Pasang OpenClaw (di shell VM)

```
curl -fsSL https://openclaw.ai/install.sh -o /tmp/install.sh
bash /tmp/install.sh
rm -f /tmp/install.sh
```

Installer memasang Node LTS dan dependensi jika belum ada, memasang OpenClaw, dan meluncurkan wizard onboarding. Lihat [Install](https://docs.openclaw.ai/id/install) untuk detailnya.

3

[Navigate to header](https://docs.openclaw.ai/id/install/azure#)

Verifikasi Gateway

Setelah onboarding selesai:

```
openclaw gateway status
```

Sebagian besar tim enterprise Azure sudah memiliki lisensi GitHub Copilot. Jika itu kasus Anda, kami merekomendasikan memilih provider GitHub Copilot di wizard onboarding OpenClaw. Lihat [provider GitHub Copilot](https://docs.openclaw.ai/id/providers/github-copilot).

## [​](https://docs.openclaw.ai/id/install/azure\#pertimbangan-biaya)  Pertimbangan biaya

Azure Bastion Standard SKU berjalan sekitar **$140/bulan** dan VM (Standard\_B2as\_v2) berjalan sekitar **$55/bulan**.Untuk mengurangi biaya:

- **Deallocate VM** saat tidak digunakan (menghentikan penagihan komputasi; biaya disk tetap ada). Gateway OpenClaw tidak akan dapat dijangkau saat VM dideallocate — mulai lagi saat Anda membutuhkannya aktif:














```
az vm deallocate -g "${RG}" -n "${VM_NAME}"
az vm start -g "${RG}" -n "${VM_NAME}"   # mulai lagi nanti
```

- **Hapus Bastion saat tidak diperlukan** dan buat ulang saat Anda membutuhkan akses SSH. Bastion adalah komponen biaya terbesar dan hanya memerlukan beberapa menit untuk provisioning.
- **Gunakan Basic Bastion SKU** (~$38/bulan) jika Anda hanya memerlukan SSH berbasis Portal dan tidak memerlukan tunneling CLI (`az network bastion ssh`).

## [​](https://docs.openclaw.ai/id/install/azure\#pembersihan)  Pembersihan

Untuk menghapus semua resource yang dibuat oleh panduan ini:

```
az group delete -n "${RG}" --yes --no-wait
```

Ini menghapus resource group dan semua yang ada di dalamnya (VM, VNet, NSG, Bastion, IP publik).

## [​](https://docs.openclaw.ai/id/install/azure\#langkah-selanjutnya)  Langkah selanjutnya

- Siapkan channel pesan: [Channels](https://docs.openclaw.ai/id/channels)
- Pair perangkat lokal sebagai node: [Nodes](https://docs.openclaw.ai/id/nodes)
- Konfigurasikan Gateway: [Konfigurasi Gateway](https://docs.openclaw.ai/id/gateway/configuration)
- Untuk detail lebih lanjut tentang deployment OpenClaw di Azure dengan provider model GitHub Copilot: [OpenClaw on Azure with GitHub Copilot](https://github.com/johnsonshi/openclaw-azure-github-copilot)

## [​](https://docs.openclaw.ai/id/install/azure\#terkait)  Terkait

- [Ikhtisar instalasi](https://docs.openclaw.ai/id/install)
- [GCP](https://docs.openclaw.ai/id/install/gcp)
- [DigitalOcean](https://docs.openclaw.ai/id/install/digitalocean)

[Podman](https://docs.openclaw.ai/id/install/podman) [DigitalOcean](https://docs.openclaw.ai/id/install/digitalocean)

Ctrl+I