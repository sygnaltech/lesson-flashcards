# Flashcard App — Getting Started

This app loads card data using `fetch()`, which means you can't just open `index.html` by double-clicking it — your browser will block it. You need to serve the files over HTTP.

Pick whichever option below matches what you have installed.

---

## VS Code — Live Server (easiest)

1. Open the project folder in VS Code
2. Install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension if you don't have it
3. Right-click `index.html` in the file explorer → **Open with Live Server**

The app will open in your browser and auto-reload whenever you save a file.

---

## Python

Python is usually pre-installed on Mac and many Windows machines.

**Check if you have it:**
```bash
python --version
# or
python3 --version
```

**Start the server:**

Mac / Linux:
```bash
cd /path/to/flashcard
python3 -m http.server 8000
```

Windows:
```bash
cd C:\path\to\flashcard
python -m http.server 8000
```

Then open **http://localhost:8000** in your browser.

---

## Node.js — npx serve

If you have Node.js installed:

```bash
npx serve /path/to/flashcard
```

It will print the URL to open (usually **http://localhost:3000**).

No install needed — `npx` downloads `serve` temporarily if you don't have it.

---

## Node.js — http-server

```bash
npx http-server /path/to/flashcard -p 8000
```

Then open **http://localhost:8000**.

---

## Ruby

```bash
cd /path/to/flashcard
ruby -run -e httpd . -p 8000
```

Then open **http://localhost:8000**.

---

## PHP

```bash
cd /path/to/flashcard
php -S localhost:8000
```

Then open **http://localhost:8000**.

---

## JetBrains IDEs (WebStorm, IntelliJ, etc.)

Right-click `index.html` in the project panel → **Open In** → **Browser**. JetBrains IDEs serve files automatically through their built-in server.

---

## Browser Extensions

If you use Chrome or Edge, the [Web Server for Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb) extension lets you serve a local folder without installing anything else.

---

## Anti-Gravity, Flux Capacitors, and Other Exotic Setups

If you're running some other environment — a custom dev container, a cloud IDE (Gitpod, CodeSandbox, GitHub Codespaces), an unusual OS, or something you built yourself in a basement — the rule is the same:

> **Serve the project folder over HTTP and open the URL in a browser.**

Any tool that does that will work. The app has no special server requirements.

---

## Troubleshooting

**The page loads but cards don't appear**
You're probably opening `index.html` directly (`file://...` in the address bar) instead of via a server (`http://localhost:...`). Use one of the methods above.

**Port already in use**
Change `8000` to any other number (`8080`, `3000`, `5500`, etc.) and update the URL accordingly.

**Nothing works**
Open your browser's developer console (F12 → Console tab) and look for a red error message. It will usually tell you exactly what went wrong.
