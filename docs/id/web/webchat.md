---
source_url: https://docs.openclaw.ai/id/web/webchat
title: "Chat Web - OpenClaw"
---

[Langsung ke konten utama](https://docs.openclaw.ai/id/web/webchat#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/id)

![ID](https://d3gk2c5xim1je2.cloudfront.net/flags/ID.svg)

Bahasa Indonesia

Cari...

Ctrl K

Cari...

Navigation

Web interfaces

Chat Web

[Get started](https://docs.openclaw.ai/id) [Install](https://docs.openclaw.ai/id/install) [Channels](https://docs.openclaw.ai/id/channels) [Agents](https://docs.openclaw.ai/id/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/id/tools) [Models](https://docs.openclaw.ai/id/providers) [Platforms](https://docs.openclaw.ai/id/platforms) [Gateway & Ops](https://docs.openclaw.ai/id/gateway) [Reference](https://docs.openclaw.ai/id/cli) [Help](https://docs.openclaw.ai/id/help)

Di halaman ini

- [Apa itu](https://docs.openclaw.ai/id/web/webchat#apa-itu)
- [Mulai cepat](https://docs.openclaw.ai/id/web/webchat#mulai-cepat)
- [Cara kerjanya (perilaku)](https://docs.openclaw.ai/id/web/webchat#cara-kerjanya-perilaku)
- [Panel alat agen Control UI](https://docs.openclaw.ai/id/web/webchat#panel-alat-agen-control-ui)
- [Penggunaan jarak jauh](https://docs.openclaw.ai/id/web/webchat#penggunaan-jarak-jauh)
- [Referensi konfigurasi (WebChat)](https://docs.openclaw.ai/id/web/webchat#referensi-konfigurasi-webchat)
- [Terkait](https://docs.openclaw.ai/id/web/webchat#terkait)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Status: UI chat SwiftUI macOS/iOS berbicara langsung ke WebSocket Gateway.

## [​](https://docs.openclaw.ai/id/web/webchat\#apa-itu)  Apa itu

- UI chat native untuk Gateway (tanpa browser tertanam dan tanpa server statis lokal).
- Menggunakan sesi dan aturan perutean yang sama seperti channel lain.
- Perutean deterministik: balasan selalu kembali ke WebChat.

## [​](https://docs.openclaw.ai/id/web/webchat\#mulai-cepat)  Mulai cepat

1. Mulai Gateway.
2. Buka UI WebChat (aplikasi macOS/iOS) atau tab chat Control UI.
3. Pastikan jalur auth Gateway yang valid telah dikonfigurasi (shared-secret secara default,
bahkan pada loopback).

## [​](https://docs.openclaw.ai/id/web/webchat\#cara-kerjanya-perilaku)  Cara kerjanya (perilaku)

- UI terhubung ke WebSocket Gateway dan menggunakan `chat.history`, `chat.send`, dan `chat.inject`.
- `chat.history` dibatasi demi stabilitas: Gateway dapat memotong field teks yang panjang, menghilangkan metadata berat, dan mengganti entri yang terlalu besar dengan `[chat.history omitted: message too large]`.
- `chat.history` mengikuti cabang transkrip aktif untuk file sesi append-only modern, sehingga cabang rewrite yang ditinggalkan dan salinan prompt yang digantikan tidak dirender di WebChat.
- Entri Compaction dirender sebagai pembatas riwayat terpadatkan yang eksplisit. Pembatas tersebut menjelaskan bahwa giliran sebelumnya dipertahankan di checkpoint dan menautkan ke kontrol checkpoint Sesi, tempat operator dapat membuat cabang atau memulihkan tampilan pra-Compaction ketika izin mereka mengizinkannya.
- Control UI mengingat `sessionId` Gateway pendukung yang dikembalikan oleh `chat.history` dan menyertakannya pada panggilan lanjutan `chat.send`, sehingga koneksi ulang dan refresh halaman melanjutkan percakapan tersimpan yang sama kecuali pengguna memulai atau mereset sesi.
- Control UI menggabungkan submit in-flight duplikat untuk sesi, pesan, dan lampiran yang sama sebelum membuat id run `chat.send` baru; Gateway tetap melakukan dedupe pada permintaan berulang yang memakai ulang kunci idempotensi yang sama.
- `chat.history` juga dinormalisasi untuk tampilan: konteks OpenClaw yang hanya runtime,
pembungkus envelope masuk, tag direktif pengiriman inline
seperti `[[reply_to_*]]` dan `[[audio_as_voice]]`, payload XML tool-call teks polos
(termasuk `<tool_call>...</tool_call>`,
`<function_call>...</function_call>`, `<tool_calls>...</tool_calls>`,
`<function_calls>...</function_calls>`, dan blok tool-call yang terpotong), serta
token kontrol model ASCII/full-width yang bocor dihapus dari teks yang terlihat,
dan entri asisten yang seluruh teks terlihatnya hanya token senyap persis
`NO_REPLY` / `no_reply` dihilangkan.
- Payload balasan yang ditandai reasoning (`isReasoning: true`) dikecualikan dari konten asisten WebChat, teks replay transkrip, dan blok konten audio, sehingga payload khusus pemikiran tidak muncul sebagai pesan asisten yang terlihat atau audio yang dapat diputar.
- `chat.inject` menambahkan catatan asisten langsung ke transkrip dan menyiarkannya ke UI (tanpa run agen).
- Run yang dibatalkan dapat tetap mempertahankan output asisten parsial yang terlihat di UI.
- Gateway mempertahankan teks asisten parsial yang dibatalkan ke riwayat transkrip ketika output yang di-buffer ada, dan menandai entri tersebut dengan metadata pembatalan.
- Riwayat selalu diambil dari Gateway (tanpa pemantauan file lokal).
- Jika Gateway tidak dapat dijangkau, WebChat bersifat hanya-baca.

## [​](https://docs.openclaw.ai/id/web/webchat\#panel-alat-agen-control-ui)  Panel alat agen Control UI

- Panel Tools Control UI `/agents` memiliki dua tampilan terpisah:

  - **Tersedia Saat Ini** menggunakan `tools.effective(sessionKey=...)` dan menampilkan apa yang benar-benar dapat digunakan
    sesi saat ini pada runtime, termasuk alat milik core, Plugin, dan channel.
  - **Konfigurasi Alat** menggunakan `tools.catalog` dan tetap berfokus pada profil, override, dan
    semantik katalog.
- Ketersediaan runtime bersifat terikat sesi. Berpindah sesi pada agen yang sama dapat mengubah daftar
**Tersedia Saat Ini**.
- Editor konfigurasi tidak menyiratkan ketersediaan runtime; akses efektif tetap mengikuti prioritas kebijakan
(`allow`/`deny`, override per agen dan provider/channel).

## [​](https://docs.openclaw.ai/id/web/webchat\#penggunaan-jarak-jauh)  Penggunaan jarak jauh

- Mode jarak jauh menyalurkan WebSocket Gateway melalui SSH/Tailscale.
- Anda tidak perlu menjalankan server WebChat terpisah.

## [​](https://docs.openclaw.ai/id/web/webchat\#referensi-konfigurasi-webchat)  Referensi konfigurasi (WebChat)

Konfigurasi lengkap: [Konfigurasi](https://docs.openclaw.ai/id/gateway/configuration)Opsi WebChat:

- `gateway.webchat.chatHistoryMaxChars`: jumlah karakter maksimum untuk field teks dalam respons `chat.history`. Ketika entri transkrip melampaui batas ini, Gateway memotong field teks yang panjang dan dapat mengganti pesan yang terlalu besar dengan placeholder. `maxChars` per permintaan juga dapat dikirim oleh klien untuk mengganti default ini untuk satu panggilan `chat.history`.

Opsi global terkait:

- `gateway.port`, `gateway.bind`: host/port WebSocket.
- `gateway.auth.mode`, `gateway.auth.token`, `gateway.auth.password`:
auth WebSocket shared-secret.
- `gateway.auth.allowTailscale`: tab chat Control UI browser dapat menggunakan header identitas Tailscale
Serve saat diaktifkan.
- `gateway.auth.mode: "trusted-proxy"`: auth reverse-proxy untuk klien browser di belakang sumber proxy **non-loopback** yang sadar identitas (lihat [Auth Proxy Tepercaya](https://docs.openclaw.ai/id/gateway/trusted-proxy-auth)).
- `gateway.remote.url`, `gateway.remote.token`, `gateway.remote.password`: target Gateway jarak jauh.
- `session.*`: penyimpanan sesi dan default kunci utama.

## [​](https://docs.openclaw.ai/id/web/webchat\#terkait)  Terkait

- [Control UI](https://docs.openclaw.ai/id/web/control-ui)
- [Dasbor](https://docs.openclaw.ai/id/web/dashboard)

[Dashboard](https://docs.openclaw.ai/id/web/dashboard) [TUI](https://docs.openclaw.ai/id/web/tui)

Ctrl+I