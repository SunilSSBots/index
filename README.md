# Google Drive Index & Token Generator

A single, clean repository to **generate Google OAuth tokens (`token.pickle`)** and **deploy a Google Drive Index using Cloudflare Workers**.

This project is designed for developers who want a **simple, reproducible, and professional workflow** to index Google Drive content without using third‑party panels.

---

## Features

- Generate `token.pickle` using Google OAuth (Drive API)
- Extract usable OAuth data from the token
- Deploy a Google Drive Index using Cloudflare Workers
- Works with:
  - My Drive
  - Shared Drives
  - Specific folders
- Clean and minimal setup
- No external paid services required

---

## Repository Structure

```
google-drive-index/
│
├── generate.py          # Generates token.pickle via OAuth login
├── unlock_token.py      # Reads token.pickle and prints auth details
├── worker.example.js    # Cloudflare Worker Drive Index template
├── credentials.json    # Google OAuth credentials (NOT included)
└── README.md
```

---

## Requirements

### System
- Python 3.8+
- pip
- Internet access

### Google Cloud
- Google Cloud Project
- Google Drive API enabled
- OAuth Client ID (Desktop App)

### Cloudflare
- Cloudflare account
- Workers enabled

---

## Step 1: Google Cloud Configuration

1. Open **Google Cloud Console**
2. Create a new project
3. Enable **Google Drive API**
4. Configure **OAuth Consent Screen**
5. Create **OAuth Client ID**
   - Application type: **Desktop App**
6. Download the credentials file
7. Rename it to:

```
credentials.json
```

Place it inside the project directory.

---

## Step 2: Generate `token.pickle`

### Clone Repository

```bash
git clone https://github.com/SunilSSBots/google-drive-index
cd google-drive-index
```

### Install Dependencies

```bash
pip install google-auth google-auth-oauthlib google-auth-httplib2
```

### Run Token Generator

```bash
python3 generate.py
```

### Authorization Flow

- A Google login URL will appear
- Open it in a browser
- Sign in to your Google account
- Allow Drive permissions
- On success, a file will be created:

```
token.pickle
```

This file stores your OAuth access and refresh tokens.

---

## Step 3: Extract Token Details (Optional but Recommended)

To use the same credentials for Cloudflare Worker authentication:

```bash
python3 unlock_token.py
```

This script helps you verify:
- Token validity
- Refresh token availability
- Linked Google account

---

## Step 4: Deploy Google Drive Index (Cloudflare Worker)

### Create Worker

1. Go to **Cloudflare Dashboard**
2. Open **Workers & Pages**
3. Create a new Worker
4. Open Worker editor

### Add Worker Code

- Open `worker.example.js`
- Copy its content
- Paste it into the Worker editor
- Save

---

## Step 5: Configure Worker Environment Variables

In **Worker Settings → Variables → Secrets**, add:

| Variable Name | Description |
|--------------|------------|
| CLIENT_ID | Google OAuth Client ID |
| CLIENT_SECRET | Google OAuth Client Secret |
| REFRESH_TOKEN | Refresh token from OAuth |
| ROOT_ID | Drive root / folder / shared drive ID |

### ROOT_ID Examples

| Use Case | Value |
|--------|------|
| My Drive | `root` |
| Shared Drive | Shared Drive ID |
| Folder Index | Folder ID |

---

## Step 6: Deploy & Verify

- Click **Deploy**
- Open the Worker URL
- Your Google Drive index should load

---

## Security Notes

- **Never upload `credentials.json` or `token.pickle` to public repositories**
- Use **Cloudflare Secrets**, not plain variables
- Revoke tokens immediately if leaked

---

## Troubleshooting

### `token.pickle` not generated
- Ensure `credentials.json` exists
- Ensure Drive API is enabled
- Complete browser authorization fully

### Worker shows Unauthorized / Blank
- Incorrect refresh token
- Wrong CLIENT_ID / CLIENT_SECRET
- Incorrect ROOT_ID
- Variable name mismatch in worker file

---

## License

MIT License

---

## Disclaimer

This project is for educational and personal use.  
You are responsible for complying with Google Drive and Cloudflare terms of service.
