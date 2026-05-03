---
source_url: https://docs.openclaw.ai/id/platforms/mac/menu-bar
title: "Bilah menu - OpenClaw"
---

[Langsung ke konten utama](https://docs.openclaw.ai/id/platforms/mac/menu-bar#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/id)

![ID](https://d3gk2c5xim1je2.cloudfront.net/flags/ID.svg)

Bahasa Indonesia

Cari...

Ctrl K

Cari...

Navigation

Setup

Bilah menu

[Get started](https://docs.openclaw.ai/id) [Install](https://docs.openclaw.ai/id/install) [Channels](https://docs.openclaw.ai/id/channels) [Agents](https://docs.openclaw.ai/id/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/id/tools) [Models](https://docs.openclaw.ai/id/providers) [Platforms](https://docs.openclaw.ai/id/platforms) [Gateway & Ops](https://docs.openclaw.ai/id/gateway) [Reference](https://docs.openclaw.ai/id/cli) [Help](https://docs.openclaw.ai/id/help)

Di halaman ini

- [Logika Status Bilah Menu](https://docs.openclaw.ai/id/platforms/mac/menu-bar#logika-status-bilah-menu)
- [Yang ditampilkan](https://docs.openclaw.ai/id/platforms/mac/menu-bar#yang-ditampilkan)
- [Model status](https://docs.openclaw.ai/id/platforms/mac/menu-bar#model-status)
- [Enum IconState (Swift)](https://docs.openclaw.ai/id/platforms/mac/menu-bar#enum-iconstate-swift)
- [ActivityKind → glif](https://docs.openclaw.ai/id/platforms/mac/menu-bar#activitykind-%E2%86%92-glif)
- [Pemetaan visual](https://docs.openclaw.ai/id/platforms/mac/menu-bar#pemetaan-visual)
- [Submenu Konteks](https://docs.openclaw.ai/id/platforms/mac/menu-bar#submenu-konteks)
- [Teks baris status (menu)](https://docs.openclaw.ai/id/platforms/mac/menu-bar#teks-baris-status-menu)
- [Ingesti peristiwa](https://docs.openclaw.ai/id/platforms/mac/menu-bar#ingesti-peristiwa)
- [Override debug](https://docs.openclaw.ai/id/platforms/mac/menu-bar#override-debug)
- [Checklist pengujian](https://docs.openclaw.ai/id/platforms/mac/menu-bar#checklist-pengujian)
- [Terkait](https://docs.openclaw.ai/id/platforms/mac/menu-bar#terkait)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#logika-status-bilah-menu)  Logika Status Bilah Menu

## [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#yang-ditampilkan)  Yang ditampilkan

- Kami menampilkan status kerja agen saat ini pada ikon bilah menu dan pada baris status pertama di menu.
- Status kesehatan disembunyikan saat pekerjaan sedang aktif; status ini kembali saat semua sesi idle.
- Submenu “Konteks” root berisi sesi terbaru alih-alih memperluasnya langsung di menu root.
- Blok “Node” di menu root hanya mencantumkan **perangkat** (node yang dipasangkan melalui `node.list`), bukan entri klien/kehadiran.
- Bagian “Penggunaan” root muncul di bawah Konteks saat snapshot penggunaan penyedia tersedia, diikuti detail biaya penggunaan jika tersedia.

## [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#model-status)  Model status

- Sesi: peristiwa datang dengan `runId` (per-run) plus `sessionKey` di payload. Sesi “utama” adalah kunci `main`; jika tidak ada, kami kembali ke sesi yang paling baru diperbarui.
- Prioritas: utama selalu menang. Jika utama aktif, statusnya langsung ditampilkan. Jika utama idle, sesi non-utama yang paling baru aktif akan ditampilkan. Kami tidak bolak-balik di tengah aktivitas; kami hanya beralih saat sesi saat ini menjadi idle atau utama menjadi aktif.
- Jenis aktivitas:
  - `job`: eksekusi perintah tingkat tinggi (`state: started|streaming|done|error`).
  - `tool`: `phase: start|result` dengan `toolName` dan `meta/args`.

## [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#enum-iconstate-swift)  Enum IconState (Swift)

- `idle`
- `workingMain(ActivityKind)`
- `workingOther(ActivityKind)`
- `overridden(ActivityKind)` (override debug)

### [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#activitykind-%E2%86%92-glif)  ActivityKind → glif

- `exec` → 💻
- `read` → 📄
- `write` → ✍️
- `edit` → 📝
- `attach` → 📎
- default → 🛠️

### [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#pemetaan-visual)  Pemetaan visual

- `idle`: makhluk normal.
- `workingMain`: badge dengan glif, tint penuh, animasi kaki “bekerja”.
- `workingOther`: badge dengan glif, tint diredam, tanpa gerakan cepat.
- `overridden`: menggunakan glif/tint yang dipilih terlepas dari aktivitas.

## [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#submenu-konteks)  Submenu Konteks

- Menu root menampilkan satu baris “Konteks” dengan jumlah/status sesi dan membuka submenu.
- Header submenu Konteks menampilkan jumlah sesi aktif selama 24 jam terakhir.
- Setiap baris sesi mempertahankan bilah token, umur, pratinjau, berpikir/verbose, reset, compact, dan tindakan hapusnya.
- Pesan pemuatan, terputus, dan kesalahan pemuatan sesi muncul di dalam submenu Konteks.
- Penggunaan penyedia dan detail biaya penggunaan tetap berada di tingkat root di bawah Konteks agar tetap dapat dilihat sekilas tanpa membuka submenu.

## [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#teks-baris-status-menu)  Teks baris status (menu)

- Saat pekerjaan aktif: `<Session role> · <activity label>`
  - Contoh: `Main · exec: pnpm test`, `Other · read: apps/macos/Sources/OpenClaw/AppState.swift`.
- Saat idle: kembali ke ringkasan kesehatan.

## [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#ingesti-peristiwa)  Ingesti peristiwa

- Sumber: peristiwa `agent` control-channel (`ControlChannel.handleAgentEvent`).
- Bidang yang diurai:
  - `stream: "job"` dengan `data.state` untuk mulai/berhenti.
  - `stream: "tool"` dengan `data.phase`, `name`, opsional `meta`/`args`.
- Label:
  - `exec`: baris pertama dari `args.command`.
  - `read`/`write`: jalur yang dipersingkat.
  - `edit`: jalur plus jenis perubahan yang disimpulkan dari jumlah `meta`/diff.
  - fallback: nama alat.

## [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#override-debug)  Override debug

- Pengaturan ▸ Debug ▸ pemilih “Override ikon”:
  - `System (auto)` (default)
  - `Working: main` (per jenis alat)
  - `Working: other` (per jenis alat)
  - `Idle`
- Disimpan melalui `@AppStorage("iconOverride")`; dipetakan ke `IconState.overridden`.

## [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#checklist-pengujian)  Checklist pengujian

- Picu job sesi utama: verifikasi ikon langsung beralih dan baris status menampilkan label utama.
- Picu job sesi non-utama saat utama idle: ikon/status menampilkan non-utama; tetap stabil hingga selesai.
- Mulai utama saat yang lain aktif: ikon langsung beralih ke utama.
- Burst alat cepat: pastikan badge tidak berkedip (tenggang TTL pada hasil alat).
- Baris kesehatan muncul kembali setelah semua sesi idle.

## [​](https://docs.openclaw.ai/id/platforms/mac/menu-bar\#terkait)  Terkait

- [aplikasi macOS](https://docs.openclaw.ai/id/platforms/macos)
- [Ikon bilah menu](https://docs.openclaw.ai/id/platforms/mac/icon)

[Penyiapan pengembangan macOS](https://docs.openclaw.ai/id/platforms/mac/dev-setup) [Ikon bilah menu](https://docs.openclaw.ai/id/platforms/mac/icon)

Ctrl+I