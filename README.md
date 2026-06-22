# IndexNow URL Submission Tool

A simple, practical IndexNow URL submission tool built for shared hosting environments — specifically developed and tested on GoDaddy shared hosting running PHP.

Built by [MacGizmoGuy](https://macgizmoguy.com) because the existing documentation for deploying IndexNow on shared hosting is genuinely incomplete, and I needed something that actually worked on my own sites.

---

## What Is IndexNow?

IndexNow is an open protocol that lets you notify search engines the moment a URL on your site is created, updated, or deleted — rather than waiting for their crawlers to find the change on their own schedule. Bing adopted it early and aggressively. In my own testing, Bing indexed submitted URLs within 24 hours of submission.

Google has been slower to embrace it, but submitting to Bing matters more than it used to — Bing now powers the web search results behind ChatGPT, making Bing indexing directly relevant to AI citation visibility.

---

## What's In This Repository

| File | Purpose |
|------|---------|
| `IndexNowSubmitter.html` | Browser-based dashboard for submitting URLs to IndexNow across two sites, with sitemap submission and submission history |
| `indexnow-proxy.php` | PHP proxy script that handles the actual API submission to IndexNow |
| `your-api-key-here.txt` | Example API key verification file (replace with your own) |

---

## How IndexNow Works

1. You generate an API key — any unique alphanumeric string
2. You place a `.txt` file named after that key in your site's root directory
3. When you submit a URL, IndexNow verifies you own the domain by fetching that key file
4. If verified, the URL is queued for expedited crawling

---

## Setup on GoDaddy Shared Hosting

### Step 1 — Generate Your API Key

Create any unique alphanumeric string. A random 32-character string works well. Keep it consistent — it never changes once set.

### Step 2 — Upload Your Key File

Create a plain text file named `your-api-key-here.txt` (where `your-api-key-here` is your actual key string) containing only the key string itself on a single line. Upload it to your site's root directory.

Verify it's accessible by visiting `https://yourdomain.com/your-api-key-here.txt` in a browser. You should see just the key string.

### Step 3 — Upload the PHP Proxy Script

Upload `indexnow-proxy.php` to your site. It can live anywhere — root, a subdirectory, doesn't matter. On GoDaddy shared hosting this works without any special server configuration.

### Step 4 — Upload the HTML Dashboard

Upload `IndexNowSubmitter.html` to the same directory as the PHP proxy script. This gives you a browser-based interface for submitting URLs and sitemaps without touching the command line.

### Step 5 — Edit the HTML Dashboard

Open `IndexNowSubmitter.html` and update the CONFIG block at the top of the script section with your own values:

```javascript
const CONFIG = {
  site1: {
    host: 'your-first-domain.com',
    key: 'your-indexnow-key-here',
    keyLocation: 'https://your-first-domain.com/your-indexnow-key-here.txt',
    sitemap: 'https://your-first-domain.com/sitemap.xml'
  },
  site2: {
    host: 'your-second-domain.com',
    key: 'your-indexnow-key-here',
    keyLocation: 'https://your-second-domain.com/your-indexnow-key-here.txt',
    sitemap: 'https://your-second-domain.com/sitemap.xml'
  }
};
```

Also update the proxy URL input field default value to match your actual proxy path.

---

## Usage

Navigate to `IndexNowSubmitter.html` in your browser. Enter one or more full URLs (including `https://`) in the relevant site panel and click Submit URLs — or click Submit Sitemap to submit your sitemap URL directly. The PHP proxy script posts to the IndexNow endpoint and returns the HTTP response code. Recent submissions are logged in the dashboard for reference.

**Response codes:**
- `200` — URL accepted
- `202` — URL accepted, key validation pending
- `400` — Bad request (check your URL format)
- `403` — Key mismatch (check your key file is accessible)
- `422` — URL doesn't belong to the host in the request
- `429` — Too many requests

---

## Verified Results

Tested on two live sites running on GoDaddy shared hosting:

- **macgizmoguy.com** — Mac technology and Apple peripherals
- **waterlilybear.com** — Container water gardening

Bing confirmed indexing of submitted URLs within 24 hours in both cases.

---

## Notes

- This tool submits to `https://api.indexnow.org/indexnow` — the canonical IndexNow endpoint. Per the protocol specification, the receiving engine automatically shares your submission with all other participating search engines within 10 seconds. One submission covers all of them.
- **Participating search engines as of 2026:** Microsoft Bing, Yandex, Naver (South Korea), Seznam.cz (Czech Republic), and Yep. Bing's adoption is particularly significant because it powers ChatGPT Search, Microsoft Copilot, and DuckDuckGo — making IndexNow directly relevant to AI citation visibility, not just traditional search.
- **Google does not support IndexNow** as of 2026. For Google indexing, continue using XML sitemaps and Google Search Console's URL Inspection tool alongside this tool.
- Your key file must remain publicly accessible at all times or submissions will fail verification.

---

## About

Built by Russell T. Baer / [MacGizmoGuy](https://macgizmoguy.com) — independent web publisher since the Mosaic era, operating on GoDaddy shared hosting like a significant portion of the independent web actually does.

*macgizmoguy.com · waterlilybear.com · Palm Springs, CA*
