---
title: 'ANURUP: privacy-first photo and PDF tools'
slug: anurup
description: Free photo and PDF tools for online application forms. Everything runs in your browser, so your files are never uploaded.
image: Anurup_background.png
date: 2026-10-27T00:00:00+05:30
weight: 1
categories:
- Web Tools
tags:
- Privacy
- PDF
- Image Processing
- WebAssembly
- PWA
- React
---

**[Open the app](https://u77w41.github.io/anurup/) · [Wiki](https://github.com/U77w41/anurup/wiki) · [GitHub Repo](https://github.com/U77w41/anurup)**

## Overview

Government exam, job, visa and KYC portals each want something slightly different: a 35×45 mm photo under 50 KB, a signature at 140×60 px, one merged PDF under 500 KB. ANURUP is a free set of photo and PDF tools for exactly these uploads.

It runs in the browser, so your files never leave your device, and there is no account to create. It works in Chrome, Edge, Firefox and Safari, on a computer or a phone, and you can install it as an app that keeps working offline.

## All tools

Each tool has its own address, so you can bookmark the ones you use most.

| Photo and signature | PDF |
| --- | --- |
| [Passport Size Photo Maker](https://u77w41.github.io/anurup/passport-photo/) | [PDF Viewer](https://u77w41.github.io/anurup/pdf-viewer/) |
| [Resize Image to KB & Pixels](https://u77w41.github.io/anurup/size-targeter/) | [JPG to PDF Converter](https://u77w41.github.io/anurup/image-to-pdf/) |
| [Compress Image to KB](https://u77w41.github.io/anurup/image-compressor/) | [Merge PDF](https://u77w41.github.io/anurup/pdf-merger/) |
| [Crop, Rotate & Flip Image](https://u77w41.github.io/anurup/crop-rotate/) | [Split PDF](https://u77w41.github.io/anurup/pdf-splitter/) |
| [Remove Image Background](https://u77w41.github.io/anurup/background-removal/) | [Organize PDF Pages](https://u77w41.github.io/anurup/pdf-page-manager/) |
| [Signature Cleaner](https://u77w41.github.io/anurup/signature-cleaner/) | [Compress PDF to KB](https://u77w41.github.io/anurup/pdf-compressor/) |
| [JPG, PNG & WebP Converter](https://u77w41.github.io/anurup/format-converter/) | [PDF to JPG / PNG](https://u77w41.github.io/anurup/pdf-to-images/) |
| | [Add Page Numbers & Watermark](https://u77w41.github.io/anurup/pdf-stamp/) |
| | [Sign & Fill PDF](https://u77w41.github.io/anurup/pdf-sign/) |
| | [Remove PDF Password](https://u77w41.github.io/anurup/pdf-unlock/) |

## How it works

1. Pick a tool on the home page, or search for the task, for example `50 KB`, `signature`, `merge` or `password`.
2. Drop a file on the upload box, click to browse, or paste an image with Ctrl/⌘+V.
3. Change the settings and click the main button. The result shows up below with a preview.
4. Download it, or send it straight to another tool.

### Chaining tools

A form upload usually takes more than one step. Every result has a **Continue with this file** row that opens the file in the next tool, so you don't have to download it and upload it again. Some common chains:

- Crop → Remove background → Passport photo → Resize to KB
- Signature Cleaner → Resize to KB → Sign & Fill PDF
- JPG to PDF → Compress PDF
- Remove PDF Password → Merge PDF → Compress PDF

Files you pass along, or save with **Save to workspace**, land in the **Workspace**. It's a shared tray that survives a page refresh and holds up to 60 files. Any upload box can pick from it, and you can select several files and send them to Merge PDF or JPG to PDF in one go.

## Privacy

ANURUP is a static website on GitHub Pages. Every tool runs inside your browser tab with Canvas, pdf-lib, PDF.js and QPDF compiled to WebAssembly, so no server ever receives your photos, PDFs or passwords.

The page downloads three things: the app itself, web fonts from Google Fonts and, the first time you use Remove Image Background, its AI model from IMG.LY's CDN. Your photo is still processed on your device.

The browser keeps your workspace files (in IndexedDB), your light or dark choice (in localStorage) and a copy of the app for offline use. A PDF password is used once to decrypt the file and isn't stored anywhere. Remove PDF Password also needs the real password. It doesn't guess or crack anything.

## Install and use offline

ANURUP is a Progressive Web App. Once installed, it opens in its own window from the dock, Start menu or home screen.

| Where | How |
| --- | --- |
| Chrome or Edge on a computer | Click **Install app** in the sidebar, or the install icon in the address bar |
| Android (Chrome) | Tap the download icon in the top bar, or **Install app** in the menu |
| iPhone and iPad (Safari) | Share → **Add to Home Screen** |
| Safari on a Mac | **File → Add to Dock** |

After your first visit the app keeps a copy of itself (about 6 MB), and every tool works without internet, installed or not. Remove Image Background is the one exception: it downloads its AI model the first time and works offline after that. When a new version is out, ANURUP shows a **Reload** prompt. Nothing changes until you click it, so a tool won't reset in the middle of a job.

## Under the hood

| Part | What it uses |
| --- | --- |
| Interface | React, TypeScript, Tailwind CSS, Radix UI |
| PDF | pdf-lib, PDF.js, and QPDF compiled to WebAssembly for removing passwords |
| Images | Canvas, and IMG.LY's background-removal model running on the device |
| Build | Vite, plus a prerender step that writes a static page for every tool, a sitemap and a 404 page |
| Offline | vite-plugin-pwa and a Workbox service worker |
| Tests | Vitest |
| Hosting | GitHub Pages, deployed by GitHub Actions on every push to `main` |

## Guides

The [ANURUP wiki](https://github.com/U77w41/anurup/wiki) has step-by-step guides:

- [Getting started](https://github.com/U77w41/anurup/wiki/Getting-Started): how the app is laid out and how to move a file from one tool to the next
- [Install as an app](https://github.com/U77w41/anurup/wiki/Install-as-an-App): add it to your dock or home screen, update it, uninstall it
- [Photo and signature guides](https://github.com/U77w41/anurup/wiki/Photo-and-Signature-Guides): resize a photo to 20-50 KB, make a passport size photo, prepare a signature for an exam form, change a background to white
- [PDF guides](https://github.com/U77w41/anurup/wiki/PDF-Guides): JPG to PDF, compress a PDF to 100 KB, merge certificates, remove the password from a bank statement or e-Aadhaar, sign a form
- [Workspace and chaining](https://github.com/U77w41/anurup/wiki/Workspace-and-Chaining): keep files between tools without downloading them
- [Privacy and security](https://github.com/U77w41/anurup/wiki/Privacy-and-Security): what stays on your device and what the app downloads
- [FAQ](https://github.com/U77w41/anurup/wiki/FAQ) and [Troubleshooting](https://github.com/U77w41/anurup/wiki/Troubleshooting)
- [Development](https://github.com/U77w41/anurup/wiki/Development): run it locally, add a tool, deploy

Found a bug? [Open an issue](https://github.com/U77w41/anurup/issues) with the tool name, your browser and what you expected to happen. Please don't attach private documents.
