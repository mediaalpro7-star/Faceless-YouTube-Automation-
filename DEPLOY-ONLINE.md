# Deploy Online

## 1. GitHub
Create a GitHub repository and upload this project.

## 2. Vercel
Import the GitHub repository into Vercel. Vercel automatically detects Next.js.

## 3. Environment Variables
Add these in Vercel Project Settings -> Environment Variables:

NEXT_PUBLIC_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_ANON_KEY
OPENAI_API_KEY
PIXABAY_API_KEY
OPENAI_TEXT_MODEL
OPENAI_TTS_VOICE

Never use NEXT_PUBLIC_ for OpenAI/Pixabay secrets.

## 4. Render Worker
The FFmpeg worker must run on a separate server/container with FFmpeg installed.
The web app can remain on Vercel.

## 5. Production architecture

Browser
  -> Vercel / Next.js
  -> Supabase
  -> OpenAI
  -> Pixabay
  -> Render Worker (FFmpeg)
  -> Supabase Storage
  -> MP4 download

The current package prepares the worker, but a production worker endpoint/queue still needs to be connected before one-click cloud rendering.
