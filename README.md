# Teams Puppeteer MCP Server

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/Node.js-%3E%3D18.0.0-green.svg)](https://nodejs.org/)
[![Model Context Protocol](https://img.shields.io/badge/MCP-Compatible-blue.svg)](https://modelcontextprotocol.io/)

A high-performance **Model Context Protocol (MCP)** server for **Microsoft Teams** chat automation, message extraction, and unread notification monitoring powered by Puppeteer.

---

## 💡 Why This Project?

Most Microsoft Teams integrations require:
- ❌ **Enterprise Azure App Registration**
- ❌ **Azure Tenant Administrator Consent**
- ❌ **Paid Microsoft 365 / Graph API licenses & Bot frameworks**

**`teams-puppeteer-mcp` takes a radically simpler approach:**
- ✅ **Zero Setup / No API Keys**: Uses `npx` with zero local file path configuration.
- ✅ **Direct Web Client Automation**: Interacts directly with Microsoft Teams (`teams.cloud.microsoft`) using your standard browser profile.
- ✅ **Persistent SSO & MFA Session**: Log in once interactively with your work, school, or personal account. Your session cookies and tokens persist locally in your user profile.
- ✅ **Zero Token Overhead**: Optimized specifically for LLMs. Returns clean, high-density human/LLM-readable text without nested JSON bloat.
- ✅ **Full Cross-Platform Support**: Works seamlessly on **Windows**, **macOS**, and **Linux**.

---

## 🚀 Quick Setup (Zero Configuration)

You do **not** need to manually clone this repository or guess local file paths. You can add it directly to your MCP client using **`npx`**.

### 1. Claude Desktop

Add this to your `claude_desktop_config.json`:
- **Windows**: `%APPDATA%\\Claude\\claude_desktop_config.json`
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Linux**: `~/.config/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "teams": {
      "command": "npx",
      "args": [
        "-y",
        "@tinobruno/teams-puppeteer-mcp"
      ]
    }
  }
}
```

> **Note**: You can also run directly from GitHub without npm:
> `"args": ["-y", "github:tinobruno/teams-puppeteer-mcp"]`

---

### 2. Cursor

Add to your Cursor MCP settings (`~/.cursor/mcp.json` or via **Settings** → **Features** → **MCP**):

```json
{
  "mcpServers": {
    "teams": {
      "command": "npx",
      "args": [
        "-y",
        "@tinobruno/teams-puppeteer-mcp"
      ]
    }
  }
}
```

---

### 3. VS Code (Cline / Roo-Code)

In your Cline MCP settings (`cline_mcp_settings.json`):

```json
{
  "mcpServers": {
    "teams": {
      "command": "npx",
      "args": [
        "-y",
        "@tinobruno/teams-puppeteer-mcp"
      ],
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

---

### 4. MinnieTheMoEcher

In `mcp_servers.json`:

```json
{
  "mcpServers": {
    "teams-puppeteer": {
      "command": "npx",
      "args": [
        "-y",
        "@tinobruno/teams-puppeteer-mcp"
      ],
      "enabled": true,
      "transport_type": "stdio"
    }
  }
}
```

---

## 🔑 First-Time Interactive Login

On your first run:
1. When your AI assistant invokes a Teams tool, a browser window will launch and navigate to Microsoft Teams (`https://teams.cloud.microsoft`).
2. Log in with your corporate SSO, password, and complete your Multi-Factor Authentication (MFA/2FA) prompt.
3. Once you reach the Teams chat interface, your session is automatically saved to your local profile directory (`~/.teams_puppeteer_profile` on macOS/Linux, or `%USERPROFILE%\\.teams_puppeteer_profile` on Windows).
4. Subsequent calls will automatically connect to your authenticated session.

---

## 🛠️ Available MCP Tools

| Tool Name | Description | Parameters |
| :--- | :--- | :--- |
| `get_last_message` | **Primary tool** to read the single latest message from a contact, group, or channel. Matches partial names automatically. | `chat_name` (required, string) |
| `get_last_unread_message` | Scans for unread messages. If `chat_name` is omitted, scans across **all** conversations and returns a compact summary. | `chat_name` (optional, string) |
| `get_last_messages` | Retrieves the last N messages from a specific conversation. | `chat_name` (required), `count` (optional, number, default: 3) |
| `list_teams_chats` | Lists sidebar conversation names, unread flags, and latest activity timestamps. | `limit` (optional, number, default: 10) |
| `send_teams_message` | Types and dispatches a message into Teams using native keyboard input into CKEditor 5. | `chat_name` (required), `message` (required) |

---

## ⚙️ Environment Variables (Optional)

You can customize the behavior by passing optional `env` variables in your MCP client config:

```json
{
  "mcpServers": {
    "teams": {
      "command": "npx",
      "args": ["-y", "@tinobruno/teams-puppeteer-mcp"],
      "env": {
        "TEAMS_HEADLESS": "true",
        "PUPPETEER_EXECUTABLE_PATH": "/usr/bin/google-chrome"
      }
    }
  }
}
```

| Variable | Description | Default |
| :--- | :--- | :--- |
| `TEAMS_HEADLESS` | Set to `"true"` to run browser in the background after initial login. | `"false"` |
| `TEAMS_PROFILE_DIR` | Custom directory path for browser cache & persistent session cookies. | `~/.teams_puppeteer_profile` |
| `PUPPETEER_EXECUTABLE_PATH` | Explicit path to your Chrome or Microsoft Edge executable. | Auto-detected |
| `DEVTOOLS_PORT` | Connect to an already running Chrome instance with remote debugging. | Auto-detected / `9222` |

---

## 🧑‍💻 Manual Installation (For Developers / Contributors)

If you are developing or modifying the server locally:

```bash
git clone https://github.com/tinobruno/teams-puppeteer-mcp.git
cd teams-puppeteer-mcp
npm install
node index.js
```

---

## 🔒 Privacy & Security

- **Local Storage Only**: Your credentials, cookies, and chat contents remain strictly on your local machine inside `.teams_puppeteer_profile/`.
- **Zero Cloud Proxies**: No data is ever routed through external third-party servers. All automation occurs directly between your local machine and Microsoft Teams endpoints.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE) - see the LICENSE file for details.
