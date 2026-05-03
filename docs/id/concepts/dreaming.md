---
source_url: https://docs.openclaw.ai/id/concepts/dreaming
title: "Dreaming - OpenClaw"
---

[Langsung ke konten utama](https://docs.openclaw.ai/id/concepts/dreaming#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/id)

![ID](https://d3gk2c5xim1je2.cloudfront.net/flags/ID.svg)

Bahasa Indonesia

Cari...

Ctrl K

Cari...

Navigation

Memory

Dreaming

[Get started](https://docs.openclaw.ai/id) [Install](https://docs.openclaw.ai/id/install) [Channels](https://docs.openclaw.ai/id/channels) [Agents](https://docs.openclaw.ai/id/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/id/tools) [Models](https://docs.openclaw.ai/id/providers) [Platforms](https://docs.openclaw.ai/id/platforms) [Gateway & Ops](https://docs.openclaw.ai/id/gateway) [Reference](https://docs.openclaw.ai/id/cli) [Help](https://docs.openclaw.ai/id/help)

Di halaman ini

- [Yang ditulis Dreaming](https://docs.openclaw.ai/id/concepts/dreaming#yang-ditulis-dreaming)
- [Model fase](https://docs.openclaw.ai/id/concepts/dreaming#model-fase)
- [Ingestion transkrip sesi](https://docs.openclaw.ai/id/concepts/dreaming#ingestion-transkrip-sesi)
- [Dream Diary](https://docs.openclaw.ai/id/concepts/dreaming#dream-diary)
- [Sinyal pemeringkatan dalam](https://docs.openclaw.ai/id/concepts/dreaming#sinyal-pemeringkatan-dalam)
- [Penjadwalan](https://docs.openclaw.ai/id/concepts/dreaming#penjadwalan)
- [Mulai cepat](https://docs.openclaw.ai/id/concepts/dreaming#mulai-cepat)
- [Perintah slash](https://docs.openclaw.ai/id/concepts/dreaming#perintah-slash)
- [Workflow CLI](https://docs.openclaw.ai/id/concepts/dreaming#workflow-cli)
- [Default utama](https://docs.openclaw.ai/id/concepts/dreaming#default-utama)
- [UI Dreams](https://docs.openclaw.ai/id/concepts/dreaming#ui-dreams)
- [Terkait](https://docs.openclaw.ai/id/concepts/dreaming#terkait)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Dreaming adalah sistem konsolidasi memori latar belakang di `memory-core`. Sistem ini membantu OpenClaw memindahkan sinyal jangka pendek yang kuat ke memori tahan lama sambil menjaga prosesnya tetap dapat dijelaskan dan ditinjau.

Dreaming bersifat **opt-in** dan dinonaktifkan secara default.

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#yang-ditulis-dreaming)  Yang ditulis Dreaming

Dreaming menyimpan dua jenis output:

- **Status mesin** di `memory/.dreams/` (penyimpanan recall, sinyal fase, checkpoint ingestion, lock).
- **Output yang dapat dibaca manusia** di `DREAMS.md` (atau `dreams.md` yang sudah ada) dan file laporan fase opsional di bawah `memory/dreaming/<phase>/YYYY-MM-DD.md`.

Promosi jangka panjang tetap hanya menulis ke `MEMORY.md`.

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#model-fase)  Model fase

Dreaming menggunakan tiga fase kooperatif:

| Fase | Tujuan | Penulisan tahan lama |
| --- | --- | --- |
| Ringan | Mengurutkan dan menyiapkan materi jangka pendek terbaru | Tidak |
| Dalam | Menilai dan mempromosikan kandidat tahan lama | Ya (`MEMORY.md`) |
| REM | Merefleksikan tema dan ide yang berulang | Tidak |

Fase-fase ini adalah detail implementasi internal, bukan “mode” terpisah yang dikonfigurasi pengguna.

Light phase

Fase ringan mengingest sinyal memori harian terbaru dan jejak recall, melakukan deduplikasi, lalu menyiapkan baris kandidat.

- Membaca dari status recall jangka pendek, file memori harian terbaru, dan transkrip sesi yang telah disunting jika tersedia.
- Menulis blok `## Light Sleep` yang dikelola saat penyimpanan menyertakan output inline.
- Mencatat sinyal penguatan untuk pemeringkatan dalam berikutnya.
- Tidak pernah menulis ke `MEMORY.md`.

Deep phase

Fase dalam memutuskan apa yang menjadi memori jangka panjang.

- Memeringkat kandidat menggunakan penilaian berbobot dan gerbang ambang batas.
- Mengharuskan `minScore`, `minRecallCount`, dan `minUniqueQueries` lulus.
- Menghidrasi ulang cuplikan dari file harian live sebelum menulis, sehingga cuplikan usang/terhapus dilewati.
- Menambahkan entri yang dipromosikan ke `MEMORY.md`.
- Menulis ringkasan `## Deep Sleep` ke `DREAMS.md` dan secara opsional menulis `memory/dreaming/deep/YYYY-MM-DD.md`.

REM phase

Fase REM mengekstrak pola dan sinyal reflektif.

- Membuat ringkasan tema dan refleksi dari jejak jangka pendek terbaru.
- Menulis blok `## REM Sleep` yang dikelola saat penyimpanan menyertakan output inline.
- Mencatat sinyal penguatan REM yang digunakan oleh pemeringkatan dalam.
- Tidak pernah menulis ke `MEMORY.md`.

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#ingestion-transkrip-sesi)  Ingestion transkrip sesi

Dreaming dapat mengingest transkrip sesi yang telah disunting ke dalam korpus Dreaming. Saat transkrip tersedia, transkrip dimasukkan ke fase ringan bersama sinyal memori harian dan jejak recall. Konten pribadi dan sensitif disunting sebelum ingestion.

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#dream-diary)  Dream Diary

Dreaming juga menyimpan **Dream Diary** naratif di `DREAMS.md`. Setelah setiap fase memiliki materi yang cukup, `memory-core` menjalankan giliran subagent latar belakang best-effort dan menambahkan entri diary pendek. Ini menggunakan model runtime default kecuali `dreaming.model` dikonfigurasi. Jika model yang dikonfigurasi tidak tersedia, Dream Diary mencoba sekali lagi dengan model default sesi.

Diary ini ditujukan untuk dibaca manusia di UI Dreams, bukan sebagai sumber promosi. Artefak diary/laporan yang dihasilkan Dreaming dikecualikan dari promosi jangka pendek. Hanya cuplikan memori yang grounded yang memenuhi syarat untuk dipromosikan ke `MEMORY.md`.

Ada juga jalur backfill historis grounded untuk pekerjaan peninjauan dan pemulihan:

Backfill commands

- `memory rem-harness --path ... --grounded` mempratinjau output diary grounded dari catatan historis `YYYY-MM-DD.md`.
- `memory rem-backfill --path ...` menulis entri diary grounded yang dapat dibalik ke `DREAMS.md`.
- `memory rem-backfill --path ... --stage-short-term` menyiapkan kandidat tahan lama grounded ke penyimpanan bukti jangka pendek yang sama yang sudah digunakan fase dalam normal.
- `memory rem-backfill --rollback` dan `--rollback-short-term` menghapus artefak backfill yang sudah disiapkan tersebut tanpa menyentuh entri diary biasa atau recall jangka pendek live.

Control UI mengekspos alur backfill/reset diary yang sama sehingga Anda dapat memeriksa hasil di scene Dreams sebelum memutuskan apakah kandidat grounded layak dipromosikan. Scene juga menampilkan jalur grounded terpisah sehingga Anda dapat melihat entri jangka pendek yang disiapkan dari replay historis, item yang dipromosikan dengan arahan grounded, dan hanya menghapus entri yang disiapkan khusus grounded tanpa menyentuh status jangka pendek live biasa.

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#sinyal-pemeringkatan-dalam)  Sinyal pemeringkatan dalam

Pemeringkatan dalam menggunakan enam sinyal dasar berbobot ditambah penguatan fase:

| Sinyal | Bobot | Deskripsi |
| --- | --- | --- |
| Frekuensi | 0.24 | Berapa banyak sinyal jangka pendek yang dikumpulkan entri |
| Relevansi | 0.30 | Kualitas retrieval rata-rata untuk entri |
| Keragaman kueri | 0.15 | Konteks kueri/hari berbeda yang memunculkannya |
| Keterkinian | 0.15 | Skor kesegaran dengan peluruhan waktu |
| Konsolidasi | 0.10 | Kekuatan kemunculan ulang lintas hari |
| Kekayaan konseptual | 0.06 | Kepadatan tag konsep dari cuplikan/path |

Hit fase ringan dan REM menambahkan boost kecil dengan peluruhan keterkinian dari `memory/.dreams/phase-signals.json`.

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#penjadwalan)  Penjadwalan

Saat diaktifkan, `memory-core` mengelola otomatis satu tugas cron untuk sweep Dreaming penuh. Setiap sweep menjalankan fase secara berurutan: ringan → REM → dalam.Sweep mencakup workspace runtime utama dan semua workspace agen yang dikonfigurasi, dengan deduplikasi berdasarkan path, sehingga fan-out workspace subagent tidak mengecualikan `DREAMS.md` dan status memori agen utama.Perilaku cadence default:

| Pengaturan | Default |
| --- | --- |
| `dreaming.frequency` | `0 3 * * *` |
| `dreaming.model` | model default |

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#mulai-cepat)  Mulai cepat

- Enable dreaming

- Custom sweep cadence


```
{
  "plugins": {
    "entries": {
      "memory-core": {
        "config": {
          "dreaming": {
            "enabled": true
          }
        }
      }
    }
  }
}
```

```
{
  "plugins": {
    "entries": {
      "memory-core": {
        "config": {
          "dreaming": {
            "enabled": true,
            "timezone": "America/Los_Angeles",
            "frequency": "0 */6 * * *"
          }
        }
      }
    }
  }
}
```

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#perintah-slash)  Perintah slash

```
/dreaming status
/dreaming on
/dreaming off
/dreaming help
```

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#workflow-cli)  Workflow CLI

- Promotion preview / apply

- Explain promotion

- REM harness preview


```
openclaw memory promote
openclaw memory promote --apply
openclaw memory promote --limit 5
openclaw memory status --deep
```

`memory promote` manual menggunakan ambang batas fase dalam secara default kecuali ditimpa dengan flag CLI.

Jelaskan mengapa kandidat tertentu akan atau tidak akan dipromosikan:

```
openclaw memory promote-explain "router vlan"
openclaw memory promote-explain "router vlan" --json
```

Pratinjau refleksi REM, kebenaran kandidat, dan output promosi dalam tanpa menulis apa pun:

```
openclaw memory rem-harness
openclaw memory rem-harness --json
```

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#default-utama)  Default utama

Semua pengaturan berada di bawah `plugins.entries.memory-core.config.dreaming`.

[​](https://docs.openclaw.ai/id/concepts/dreaming#param-enabled)

enabled

boolean

default:"false"

Aktifkan atau nonaktifkan sweep Dreaming.

[​](https://docs.openclaw.ai/id/concepts/dreaming#param-frequency)

frequency

string

default:"0 3 \* \* \*"

Cadence Cron untuk sweep Dreaming penuh.

[​](https://docs.openclaw.ai/id/concepts/dreaming#param-model)

model

string

Override model subagent Dream Diary opsional. Gunakan nilai `provider/model` kanonis saat juga menetapkan allowlist `allowedModels` subagent.

`dreaming.model` memerlukan `plugins.entries.memory-core.subagent.allowModelOverride: true`. Untuk membatasinya, tetapkan juga `plugins.entries.memory-core.subagent.allowedModels`. Kegagalan kepercayaan atau allowlist tetap terlihat alih-alih fallback secara diam-diam; percobaan ulang hanya mencakup error model-tidak-tersedia.

Kebijakan fase, ambang batas, dan perilaku penyimpanan adalah detail implementasi internal (bukan konfigurasi yang ditampilkan kepada pengguna). Lihat [Referensi konfigurasi memori](https://docs.openclaw.ai/id/reference/memory-config#dreaming) untuk daftar key lengkap.

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#ui-dreams)  UI Dreams

Saat diaktifkan, tab **Dreams** Gateway menampilkan:

- status aktif Dreaming saat ini
- status tingkat fase dan keberadaan sweep yang dikelola
- jumlah jangka pendek, grounded, sinyal, dan dipromosikan-hari-ini
- waktu run terjadwal berikutnya
- jalur Scene grounded terpisah untuk entri replay historis yang disiapkan
- pembaca Dream Diary yang dapat diperluas dan didukung oleh `doctor.memory.dreamDiary`

## [​](https://docs.openclaw.ai/id/concepts/dreaming\#terkait)  Terkait

- [Memori](https://docs.openclaw.ai/id/concepts/memory)
- [CLI Memori](https://docs.openclaw.ai/id/cli/memory)
- [Referensi konfigurasi memori](https://docs.openclaw.ai/id/reference/memory-config)
- [Pencarian memori](https://docs.openclaw.ai/id/concepts/memory-search)

[Commitments](https://docs.openclaw.ai/id/concepts/commitments) [Compaction](https://docs.openclaw.ai/id/concepts/compaction)

Ctrl+I