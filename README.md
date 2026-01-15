
<!-- =========================================================
  Google Drive Index + token.pickle Generator (Single Repo)
  Repo: https://github.com/SunilSSBots/google-drive-index
========================================================== -->

<div align="center" style="padding: 24px 16px; border-radius: 18px; background: linear-gradient(135deg, #111827 0%, #0b1220 60%, #0f172a 100%); color: #e5e7eb; border: 1px solid #1f2937;">
  <h1 style="margin: 0; font-size: 34px; letter-spacing: .5px;">Google Drive Index + Token Generator</h1>
  <p style="margin: 10px 0 0; font-size: 15px; color: #cbd5e1;">
    Ek hi repo se <b>token.pickle</b> generate karein aur usi access se <b>Google Drive Index</b> (Cloudflare Worker) deploy karein.
  </p>

  <p style="margin-top: 18px;">
    <img alt="License" src="https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge">
    <img alt="Python" src="https://img.shields.io/badge/Python-3.x-3b82f6?style=for-the-badge">
    <img alt="Cloudflare Workers" src="https://img.shields.io/badge/Cloudflare-Workers-f97316?style=for-the-badge">
  </p>
</div>

<br/>

## ✨ What this repo does

### 1) `token.pickle` Generator (Python)
- `credentials.json` (Google OAuth client) ka use karke Google login + consent flow chalata hai
- First time authorize karne par `token.pickle` create hota hai

### 2) Google Drive Index (Cloudflare Workers)
- `worker.example.js` ko Cloudflare Worker me deploy karke Drive listing/index ban sakta hai
- Worker ko authenticate karne ke liye aapko OAuth details/refresh token chahiye (same credentials + token flow se)

---

## 📁 Repo Structure

| File | Purpose |
|------|---------|
| `generate.py` | OAuth login flow run karke `token.pickle` banata hai |
| `unlock_token.py` | `token.pickle` se useful auth values nikalne/print karne ke kaam aata hai (refresh token / token details) |
| `worker.example.js` | Cloudflare Worker template (Drive Index) |
| `README.md` | Documentation |

> NOTE: `token.pickle` sensitive hota hai — public repo me upload **mat** karein.

---

## ✅ Requirements

### A) Google Cloud Setup
1. Google Cloud Console me **New Project** banayein
2. **Google Drive API** enable karein
3. **OAuth Consent Screen** configure karein
4. **Create Credentials → OAuth Client ID**
   - Type: **Desktop App** (recommended for local/termux auth flow)
5. `credentials.json` download karein
6. Us file ka naam **exactly** `credentials.json` rakhein

### B) System Requirements
- Python 3.x
- pip

---

## 🚀 Step-by-step: `token.pickle` Generate (Termux / Linux / Windows)

### 1) Repo clone
```bash
git clone https://github.com/SunilSSBots/google-drive-index
cd google-drive-index

2) Python deps install
pip install google-auth google-auth-oauthlib google-auth-httplib2

3) credentials.json place karein

credentials.json ko isi folder me copy kar dein:

google-drive-index/credentials.json

4) Token generate karein
python3 generate.py


Expected flow:

Terminal me ek URL aayega

Browser me open karke Google login karein

Allow permissions

Success message: “authentication flow has completed…”

5) Output verify

Isi folder me file ban jaani chahiye:

✅ token.pickle

📲 (Android/Termux) token.pickle ko browser se download ka easy method

Agar aap mobile me kaam kar rahe ho aur file share/download karni ho:

python3 -m http.server 8080


Phir browser me open:

http://localhost:8080

Waha se token.pickle download kar sakte ho.

🔓 Step-by-step: token.pickle se Worker/Index ke liye values nikalna

Cloudflare Worker ko usually in cheezon ki zarurat hoti hai:

OAuth Client ID

OAuth Client Secret

Refresh Token

Drive Root/Folder/Shared Drive ID

1) Token details extract/run
python3 unlock_token.py


Goal: is script se aapko refresh token (ya auth details) mil jaayengi jinko aap Worker me secrets/vars me set karoge.

Tip: Agar output me refresh token / client details clearly na milein, ensure karein:

credentials.json same folder me hai

token.pickle same folder me hai

aapne correct Google account se authorize kiya hai

🌐 Step-by-step: Google Drive Index Deploy (Cloudflare Workers)
1) Cloudflare account + Workers setup

Cloudflare dashboard → Workers & Pages

Create Worker

2) Worker code paste

Repo ka worker.example.js open karein

Uska content copy karke Cloudflare Worker editor me paste karein

Save

3) Required config set karein (ENV/Secrets)

Cloudflare Worker me aapko env vars / secrets set karne honge (names worker script me defined hote hain). Typical pattern:

Workers → Settings → Variables

Add Secrets (recommended)

CLIENT_ID

CLIENT_SECRET

REFRESH_TOKEN

Also set Drive target:

ROOT_ID

My Drive root ke liye aksar root

Shared Drive/Folder ke liye uska ID

NOTE: Exact variable names worker.example.js ke andar defined hote hain.
Aapko wahi names rakhne hain jo script expect karta hai.

4) Deploy

Save & Deploy

Worker URL open karke verify karein

🧠 How to choose ROOT_ID (Important)

My Drive (Personal): root

Shared Drive: Shared Drive ka root ID

Specific Folder: Folder ID

Best practice: Shared Drive ya root use karein; folder-only mode me search limitations ho sakti hain (Drive API behavior).

🔒 Security Notes (Must Read)

credentials.json + token.pickle private rakhein

Public GitHub repo me upload na karein

Cloudflare Worker secrets hamesha Secrets me store karein (plain variables me nahi)

🧰 Troubleshooting
token.pickle create nahi ho raha

Ensure credentials.json same folder me hai

Ensure Google Drive API enabled hai

First run me browser consent complete karein

Worker blank / unauthorized

Refresh token / client id/secret wrong hai

Wrong ROOT_ID

Secrets names mismatch (worker file me expected names check karein)

📜 License

MIT

<br/> <div align="center" style="color:#94a3b8;"> Made for easy OAuth token generation + Drive Index deployment. </div> ```
