# Faceless Shorts Automation V1

Starter MVP untuk membuat YouTube Shorts faceless.

## Fitur
- Login Supabase
- Dashboard project
- Input topik, durasi, bahasa, gaya
- Penyimpanan project ke Supabase
- AI script generator via server-side API
- Mode demo jika OPENAI_API_KEY belum tersedia

## Setup
1. Install Node.js 20+.
2. Ekstrak project.
3. Jalankan `npm install`.
4. Salin `.env.example` menjadi `.env.local`.
5. Isi:
   - NEXT_PUBLIC_SUPABASE_URL
   - NEXT_PUBLIC_SUPABASE_ANON_KEY
   - OPENAI_API_KEY (opsional untuk mode demo)
6. Jalankan `npm run dev`.
7. Buka http://localhost:3000

## Supabase
Gunakan SQL database yang sudah dibuat pada percakapan ini.

## Catatan
OPENAI_API_KEY hanya boleh berada di server (`.env.local` / environment variables Vercel), bukan `NEXT_PUBLIC_*`.

Tahap berikutnya:
- pencarian footage berlisensi
- TTS
- subtitle
- FFmpeg render 1080x1920
- storage output MP4
- batch generation


## V2: Pencarian Footage Pixabay

Tambahkan ke `.env.local`:

`PIXABAY_API_KEY=API_KEY_PIXABAY_ANDA`

Setelah aplikasi berjalan, buka `/footage` untuk mencari footage berdasarkan kata kunci.


## V3: Voice-over & Subtitle

Tambahkan `OPENAI_API_KEY` ke `.env.local`.
- `/voice-over` membuat MP3 voice-over dari script.
- `/subtitles` membuat file WebVTT sederhana berdasarkan durasi video.
- `/footage` mencari footage Pixabay.

Catatan: tahap render MP4 membutuhkan worker/server dengan FFmpeg; V3 ini menyiapkan komponen input/output sebelum render worker produksi.


## V4: FFmpeg Render Worker

V4 menghasilkan video Shorts 1080x1920 (9:16) dari footage + voice-over + subtitle.
Prasyarat mesin render: FFmpeg harus tersedia sebagai perintah `ffmpeg`.

Langkah:
1. Salin `worker/job.example.json` menjadi `worker/job.json`.
2. Letakkan footage, MP3 voice-over, dan VTT subtitle di `worker/input/`.
3. Jalankan `npm run render`.
4. Hasil: `worker/output/shorts-final.mp4`.

Untuk deployment online, Render Worker sebaiknya dijalankan pada server/container yang mendukung FFmpeg; aplikasi web dapat tetap di-deploy terpisah.


## V5: One-command Shorts Automation

Set `.env.local`:
- `OPENAI_API_KEY=...`
- `PIXABAY_API_KEY=...`
- `OPENAI_TEXT_MODEL=gpt-5.6-luna`
- `OPENAI_TTS_VOICE=coral`

Install FFmpeg on the render machine, then run:

`npm run shorts -- "5 fakta unik tentang Jepang"`

Pipeline:
1. Generate script
2. Search/download Pixabay footage
3. Generate OpenAI TTS voice-over
4. Generate VTT subtitles
5. Render 1080x1920 MP4

Output: `worker/output/shorts-final.mp4`

Important: only publish footage you are permitted to reuse and verify the applicable Pixabay license/terms for your intended use.


## V6: Online Deployment Structure

V6 includes deployment notes for Vercel + a separate FFmpeg Render Worker.
Keep OpenAI and Pixabay keys server-side; do not prefix them with `NEXT_PUBLIC_`.
