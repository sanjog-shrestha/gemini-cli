# Zapier MCP: GitHub & Gemini CLI — Practical Exercise Guide

> A hands-on guide to connecting Gemini CLI with GitHub and Zapier MCP on Windows.
---

## Prerequisites

- Windows 10/11
- Node.js 20+ ([nodejs.org](https://nodejs.org))
- A GitHub account
- A Zapier account ([zapier.com](https://zapier.com))
- A Google account (for Gemini CLI authentication)

---

## Phase 1 — Installation

### 1. Install Node.js 20+

Download and run the Windows installer from [nodejs.org](https://nodejs.org). Verify in PowerShell:

```powershell
node --version   # must be 20+
npm --version
```

### 2. Install Gemini CLI

Open **PowerShell as Administrator** and run:

```powershell
npm install -g @google/gemini-cli
```

Verify:

```powershell
gemini --version
```


### 3. Authenticate Gemini CLI

Run `gemini` in any terminal. On first launch it opens a browser window — sign in with your Google account.

> **Settings file location on Windows:** `C:\Users\YourName\.gemini\settings.json`

---

## Phase 2 — Connect GitHub MCP Server

### 4. Create a GitHub Personal Access Token (PAT)

Go to `GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens`.

Grant the following permissions:

| Permission      | Access         |
|-----------------|----------------|
| Metadata        | Read           |
| Contents        | Read & Write   |
| Issues          | Read & Write   |
| Pull Requests   | Read & Write   |

Copy the generated token — you will only see it once.

### 5. Edit settings.json

Open `C:\Users\YourName\.gemini\settings.json` and add the `mcpServers` block:

```json
{
  "selectedAuthType": "gemini-api-key",
  "mcpServers": {
    "github": {
      "httpUrl": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer YOUR_GITHUB_PAT_HERE"
      },
      "timeout": 5000
    }
  }
}
```

> ⚠️ Do not share this file. The PAT grants access to your GitHub repositories.

### 6. Verify GitHub MCP tools loaded

Restart Gemini CLI, then type:

```
/mcp
```

You should see GitHub tools listed such as `list_repos`, `create_issue`, `get_pull_request`, and more.
<img width="917" height="943" alt="image" src="https://github.com/user-attachments/assets/939b81ce-1976-4ceb-b682-7e7d9d9514c5" />

---

## Phase 3 — Connect Zapier MCP Server

### 7. Get your Zapier MCP server URL

1. Log in at [zapier.com/mcp](https://zapier.com/mcp)
2. Click **+ New MCP Server**
3. Search for and select **Gemini CLI** as your AI agent
4. Add the apps you want to connect (GitHub, Gmail, Slack, etc.)
<img width="1906" height="746" alt="image" src="https://github.com/user-attachments/assets/2c8d2f5a-5b80-4d07-a9a0-846d3b542f63" />


### 8. Authenticate Zapier via OAuth

Zapier uses OAuth for authentication. Run this command in your terminal:

```powershell
gemini mcp add --scope user zapier https://mcp.zapier.com/api/v1/connect --type http
```

Then start Gemini CLI and run:

```
/mcp auth zapier
```
<img width="1457" height="671" alt="image" src="https://github.com/user-attachments/assets/3c9d51db-d159-43f2-b043-dd99e85c83bf" />

A browser window will open — authorize Zapier with your account. The OAuth token is saved automatically.

### 9. Verify both servers are connected

Run `/mcp` inside Gemini CLI. You should see:

```
🟢 github  — Connected
🟢 zapier  — Connected
```
<img width="728" height="478" alt="image" src="https://github.com/user-attachments/assets/064cee20-c0a8-415e-8edd-7528ba0e595e" />

---

## Phase 4 — Practical Exercises

### Exercise A — GitHub: List & create an issue

```
List the open issues in my repo YOUR_USERNAME/YOUR_REPO
```
<img width="902" height="188" alt="image" src="https://github.com/user-attachments/assets/7293f80f-eab8-4bf9-9d57-1e5a4b527cfd" />

Then:


```
Create a new issue titled "Test MCP integration" with body
"Created via Gemini CLI + GitHub MCP" in my repo YOUR_USERNAME/YOUR_REPO
```
<img width="900" height="336" alt="image" src="https://github.com/user-attachments/assets/23d12a0a-7611-4f75-bf9e-5850662865b9" />
Verify the issue appears on GitHub.com.
<img width="1897" height="743" alt="image" src="https://github.com/user-attachments/assets/623f068f-8bd1-4dad-91af-6c2d9b769a69" />

---

### Exercise B — Zapier: Trigger an action


For Gmail:

```
Draft an email to test@example.com with subject "MCP Test" and body "Sent via Gemini CLI"
```
<img width="692" height="630" alt="image" src="https://github.com/user-attachments/assets/14b11159-5ec2-4c9c-8fae-35071855755a" />

---

### Exercise C — Chain both MCPs in one prompt

```
Look at the open issues in my repo YOUR_USERNAME/YOUR_REPO.
For each issue labeled "bug", send me a summary as a Slack message in #dev-alerts.
```

Gemini will call the GitHub MCP to fetch issues, then call the Zapier MCP to send the Slack messages — no code required.
<img width="900" height="315" alt="image" src="https://github.com/user-attachments/assets/ec5d5641-e412-4c02-ab7a-b834fbdafa8e" />
<img width="1348" height="362" alt="image" src="https://github.com/user-attachments/assets/58b65f41-be48-4299-a47d-578062672a9d" />


---

## Key Concepts

- **MCP (Model Context Protocol):** A standardized protocol that lets AI agents securely interact with external tools and services.
- **Zapier MCP:** Connects your AI agent to 9,000+ apps without writing integration code.
- **GitHub MCP:** Lets Gemini CLI read and write issues, pull requests, and repository content.
- **settings.json:** The Gemini CLI configuration file where all MCP servers are registered.

---

## Resources

- [Gemini CLI GitHub](https://github.com/google-gemini/gemini-cli)
- [Zapier MCP](https://zapier.com/mcp)
- [GitHub MCP Server](https://github.com/github/github-mcp-server)
- [MCP Protocol](https://modelcontextprotocol.io)
