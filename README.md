# AI-Powered LinkedIn Post Automation

## Overview

AI-Powered LinkedIn Post Automation is a developer-focused tool that helps users quickly generate high‑quality LinkedIn posts using artificial intelligence. The system allows users to input a **topic/prompt**, select a **tone**, and define a **goal**. The backend then generates a structured LinkedIn post including:

- Post content
- Hashtags
- Call-to-action (CTA)

The tool is designed to integrate with a **Chrome extension interface** so users can generate content and insert it directly into the LinkedIn editor.

The backend is built with:

- Node.js
- Express.js
- PostgreSQL
- OpenAI API
- Groq API (primary provider with OpenAI fallback)

The architecture follows modern backend best practices with clearly separated layers for:

- routes
- controllers
- services
- validators
- database utilities

---

# Key Features

• AI‑generated LinkedIn posts  
• Tone and goal customization  
• Structured output (post, hashtags, CTA)  
• Groq-first generation with OpenAI fallback  
• Draft storage capability  
• Usage analytics tracking  
• PostgreSQL database persistence  
• Secure authentication using JWT  
• Production‑ready Node.js architecture  

---

# Project Architecture

```
AI-POWERED_LINKEDIN_POST_AUTOMATION/

├── .agents/skills/neon-postgres/
├── .github/
├── extension/
│   ├── icons/
│   ├── utils/
│   ├── background.js
│   ├── content.js
│   ├── manifest.json
│   ├── popup.html
│   ├── popup.css
│   └── popup.js
│
├── server/
│   ├── node_modules/
│   ├── scripts/
│   ├── src/
│   │   ├── config/
│   │   ├── controllers/
│   │   ├── db/
│   │   ├── middlewares/
│   │   ├── routes/
│   │   ├── services/
│   │   ├── utils/
│   │   ├── validators/
│   │   ├── app.js
│   │   └── server.js
│   │
│   ├── .env                # gitignored
│   ├── .prettierignore
│   ├── .prettierrc
│   ├── eslint.config.mjs
│   ├── google_auth.json    # gitignored
│   ├── package-lock.json
│   ├── package.json
│   └── node_modules/       # gitignored
│
├── contract_doc.md
├── extension_private.pem   # gitignored
├── package-lock.json
├── README.md
├── skills-lock.json
├── Task-breakdown-2.docx
├── Task-breakdown.docx
└── Technical_requirements-Linkedin-chrome-extension.docx
```

---

# API Contract & Coding Standards

The file:

```
contract_doc.md
```

contains:

• The complete API contract for all backend endpoints  
• Request and response structures  
• Database field standards  
• Industry‑standard Node.js coding conventions  
• Team Git workflow guidelines  

All contributors **must follow the conventions described in `contract_doc.md`.**

---

# Requirements

Install the following on your machine before running the project:

• Node.js (LTS recommended)  
• PostgreSQL  
• Git  

Check installation:

```bash
node -v
npm -v
```

---

# Project Setup (For New Developers)

Clone the repository:

```bash
git clone https://github.com/JasperZeroes/ai-powered_linkedin_post_automation.git
cd ai-powered_linkedin_post_automation
```

Install dependencies:

```bash
npm install
```

## Environment Variables

Create a `.env` file in the project root which is also the `server` folder.

Example:
```
PORT=5000

DATABASE_URL=postgresql://username:password@localhost:5432/linkedin_chrome_extension

OPENAI_API_KEY=your_openai_key (You don't need this for now)
GROQ_API_KEY=your_groq_key (You don't need this for now)

JWT_SECRET=your_jwt_secret (You don't need this for now)
```

Never commit `.env` to Git.

## Google OAuth 2.0 Cloud (Start from Scratch)

Go to Google Cloud > Google Auth Platform: [example](https://console.cloud.google.com/auth/verification?project=nexusbyte-linkedin-post-gen).

Create a new project and choose it.

Create a new client under **Clients**. For **Application Type**,
- Choose `Web application` for development.
- Choose `Chrome extension` for deployment (after being deployed in Chrome extension store).

## Google OAuth 2.0 Cloud (Use Someone's Configuration)

The above steps are unnecessary when using a preconfigured client on the Google Auth Platform.

Create `google_auth.json` file in the `server` folder.

Example for `Web application`:
```json
{
    "web":{
        "client_id":"example_string.apps.googleusercontent.com",
        "project_id":"nexusbyte-linkedin-post-gen",
        "auth_uri":"https://accounts.google.com/o/oauth2/auth",
        "token_uri":"https://oauth2.googleapis.com/token",
        "auth_provider_x509_cert_url":"https://www.googleapis.com/oauth2/v1/certs",
        "client_secret":"example_string",
        "redirect_uris":["https://example_string.chromiumapp.org/"]
    }
}
```

Example for `Chrome extension`:
```json
{
    "installed": {
        "client_id":"example_string.apps.googleusercontent.com",
        "project_id":"nexusbyte-linkedin-post-gen",
        "auth_uri":"https://accounts.google.com/o/oauth2/auth",
        "token_uri":"https://oauth2.googleapis.com/token",
        "auth_provider_x509_cert_url":"https://www.googleapis.com/oauth2/v1/certs"
    }
}
```

Install extension based on the [guide](#chrome-extension-setup).

Go to `chrome://extensions/` to copy the extension ID.

Wrap the ID in this format:
```
# replace placeholder with actual ID string
https://example_string.chromiumapp.org/
```

Add that URI to **Authorized redirect URIs** in Google Cloud > Google Auth Platform.

Add that URI to `google_auth.json` > `"redirected_uris":[]`.


---

# Running Database Migrations

This project includes a SQL schema file that defines the database structure.

Tables created:

- users
- drafts
- generated_posts
- usage_events

Run the migration script:

```bash
npm run migrate
```

or

```bash
node src/db/migrate.js
```

This will execute the SQL inside:

```
src/db/schema.sql
```

---

# Testing Database Connection

To verify that the database connection works correctly, run:

```bash
node src/db/test-connection.js
```

* NB: Make sure you're inside the server folder to verify database connection.


Expected output:

```output
Database connection successful
Current DB time: ...
```

If the connection fails, verify:

- PostgreSQL is running
- DATABASE_URL is correct
- credentials are valid e.g database name and password

---

# Syncing Extension Google Client ID

To ensure the Chrome extension uses the correct Google OAuth client ID during development, run:

```bash
npm run sync-extension-google-client
```

**only when**:
- Google OAuth client is updated in `google_auth.json`
- Switching between different Google Cloud projects or client configurations

**Do not need to run this** every time you start the server or make regular code changes.

The above command runs:

```
node scripts/sync-extension-google-client.js
```

which reads the client ID from your `google_auth.json` config and updates the Chrome extension's OAuth credentials automatically.

Expected output:

```output
Updated extension/manifest.json oauth2.client_id from google_auth.json
```

---

# Running the Development Server

Start the server using:

```bash
npm run dev
```

This runs:

```
nodemon src/server.js
```

Nodemon automatically restarts the server when files change.

Example output:

```output
Server running on port 5000
```

---

# Starting the Server in Production

For production environments run:

```bash
npm start
```

This runs:

```
node src/server.js
```

---

# Health Check Endpoint

You can test if the backend is running by calling the health endpoint.

Open your browser or Postman and visit:

```
http://localhost:5000/health
```

Expected response:

```output
{
  "success": true,
  "message": "Server is running"
}
```

---

# Development Scripts

Available npm scripts:

```
npm run dev                           → Start development server with nodemon
npm start                             → Start production server
npm run migrate                       → Run database migrations
npm run lint                          → Run ESLint
npm run lint:fix                      → Auto fix lint issues
npm run format                        → Format code using Prettier
npm run sync-extension-google-client  → Sync extension's client id from Google OAuth credentials
```

---

# Chrome Extension Setup

To run the popup UI in Chrome:

## Step 1: Open Extensions Page

* Type this in the Chrome search bar:
  ```
  chrome://extensions/
  ```

## Step 2: Enable Developer Mode

* Toggle **Developer mode** (top right) or Press F12

## Step 3: Load Extension

* Click **Load unpacked**
* Select the folder:
  ```
  /extension
  ```

## Step 4: Pin the Extension

* Click the puzzle icon in Chrome toolbar
* Pin **AI LinkedIn Post Generator**

## Step 5: Test the Popup

* Click the extension icon
* You should see:

  * Auth screen (signup/login)
  * Then generator UI after login

## ⚠️ Important Notes

* Any changes to extension files require clicking **Reload** in `chrome://extensions/`
* Make sure:
  * `manifest.json` is valid
  * No console errors in popup

---

# Security Practices

This project includes:

• Helmet for HTTP security headers  
• JWT authentication  
• Input validation  
• Structured error responses  

Never expose:

- API keys
- database credentials
- JWT secrets

---

# Contributing

Before contributing:

1. Pull the latest changes
2. Follow the coding conventions in `contract_doc.md`
3. Run linting before committing
4. Use feature branches

Example branch name:

```
feature/post-generation-endpoint
```

Commit style:

```
feat: add draft saving endpoint
fix: correct validation logic
docs: update readme
```

---

# Future Improvements

Planned improvements include:

• Chrome extension publishing  
• LinkedIn auto‑insertion improvements  
• Draft editing interface  
• Analytics dashboard  
• AI cost monitoring  
• Team workspaces  

---

# Maintainers

Project maintained by the development team building the **AI‑Powered LinkedIn Post Automation Tool**.

# Ai-Powered_Linkedin_Post_Automation
>>>>>>> 2bfa95967bd957a3e5ea353486da7b0adc721f11

=======
# -NexusByte-Project-Ai-Powered_Linkedin2
>>>>>>> 0014348469d30627c42d404045846157ba18eb05
