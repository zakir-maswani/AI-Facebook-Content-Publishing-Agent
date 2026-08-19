<div align="center">

# 📘 AI-Powered Facebook Content Publishing System

**An n8n workflow that turns a topic into an AI-written caption, an AI-generated poster image, human approval via email, and automatic publishing to Facebook.**

![n8n](https://img.shields.io/badge/n8n-workflow-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)
![Gemini](https://img.shields.io/badge/Google%20Gemini-AI-4285F4?style=for-the-badge&logo=googlegemini&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-Llama%203.1-F55036?style=for-the-badge&logo=groq&logoColor=white)
![Hugging Face](https://img.shields.io/badge/Hugging%20Face-Image%20Gen-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![Facebook](https://img.shields.io/badge/Facebook-Graph%20API-1877F2?style=for-the-badge&logo=facebook&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

</div>

---

> ### ⚠️ Reconstruction Notice
> The original workflow JSON file was lost. This version was **rebuilt from a screenshot** of the n8n canvas, based on the visible node names and connections. Node logic, AI prompts, field mappings, and API parameters have been **reasonably reconstructed to match the intended purpose** — they are not recovered originals. Review and adjust every node before using this in production.

## 📋 Overview

This n8n workflow automates end-to-end Facebook content creation: a topic is submitted via form, an AI agent drafts a caption, a human approves or rejects it by email, an approved caption gets turned into an AI-generated poster image, and the final post is published directly to a Facebook Page.

## 🔄 How It Works

```
📝 Form Submission
   → 🤖 AI Agent (Gemini) — draft caption
   → ✏️ Edit Fields
   → 📧 Send & Wait for Approval (Gmail)
   → 🔀 Text Classifier (approved / rejected)
        ├── ✅ Approved
        │     → 🎨 Poster Prompt Generator (Groq)
        │     → 🖼️ Hugging Face API — generate image
        │     → ☁️ Upload to Google Drive
        │     → 🔗 Share file (public link)
        │     → 📤 Facebook Graph API — publish post
        └── ❌ Rejected
              → 🤖 AI Agent1 (Gemini + SerpAPI) — regenerate caption
              → (loops back to approval email)
```

| Node | Purpose |
|------|---------|
| **On form submission** | Collects a content topic (and optional notes) to kick off the workflow |
| **AI Agent** (Gemini + Memory) | Drafts the initial Facebook caption |
| **Edit Fields** | Normalizes/stores the draft caption and topic for later steps |
| **Send message and wait for response** | Emails the draft to a reviewer and pauses until they reply (Gmail "send and wait") |
| **Text Classifier** | Reads the reviewer's reply and routes to *approved* or *rejected* |
| **Poster prompt generator** (Groq) | Converts the approved caption into a descriptive image-generation prompt |
| **Hugging Face API** | Generates a poster image from the prompt (Stable Diffusion) |
| **Upload file / Share file** (Google Drive) | Stores the generated image and makes it publicly accessible |
| **Facebook Graph API** | Publishes the final caption + image link to a Facebook Page |
| **AI Agent1** (Gemini + SerpAPI) | If rejected, researches context and rewrites the caption based on feedback, looping back for re-approval |

## ✨ Features

- 📝 **Simple intake** — one form to kick off the whole pipeline
- 🤖 **AI-drafted captions** — Gemini writes the first pass
- 👀 **Human-in-the-loop approval** — nothing publishes without sign-off
- 🎨 **Auto-generated visuals** — AI turns the caption into a matching poster image
- 🔁 **Feedback loop** — rejected drafts get automatically revised and resent for approval
- 📤 **Direct publishing** — approved posts go straight to Facebook, no manual copy-paste

## 🧰 Requirements

- An [n8n](https://n8n.io) instance (self-hosted or cloud)
- **Google Gemini API** credential
- **Groq API** credential
- **Gmail OAuth2** credential
- **Hugging Face** API token (Inference API access)
- **Google Drive OAuth2** credential
- **Facebook Graph API** credential (Page access token with `pages_manage_posts` permission)
- **SerpAPI** credential (for the rejection/research loop)

## 🚀 Setup Instructions

1. **Import the workflow**
   In n8n, go to **Workflows → Import from File** and select [`AI-Powered_Facebook_Content_Publishing_System.json`](./AI-Powered_Facebook_Content_Publishing_System.json).

2. **Reconnect every credential** — Gemini, Groq, Gmail, Google Drive, Facebook Graph API, and SerpAPI all need to be pointed at your own accounts.

3. **Review and rewrite the AI prompts** in the `AI Agent`, `AI Agent1`, and `Poster prompt generator` nodes — these were reconstructed and should reflect your actual brand voice and content rules.

4. **Set your Facebook Page ID** in the `Facebook Graph API` node.

5. **Set your Google Drive folder** in the `Upload file` node.

6. **Set your Hugging Face model endpoint and token** in the `Hugging Face API` node (defaults to Stable Diffusion XL — swap for any image model you prefer).

7. **Test end-to-end** with `Execute workflow` before activating, especially the approval email → classifier → publish path.

8. **Activate the workflow** once you've verified each step works as expected.

## 📁 Repository Structure

```
├── AI-Powered_Facebook_Content_Publishing_System.json   # Reconstructed n8n workflow (needs configuration)
├── README.md                                             # You are here
└── LICENSE                                                # MIT License
```

## 🔒 Security Note

All credential IDs, API tokens, page IDs, and folder IDs in this file are placeholders. **Never commit real API keys, access tokens, or credential IDs to this repo.**

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE) file for details.

---

<div align="center">
Made with ❤️ using n8n + Gemini + Groq + Hugging Face
</div>
