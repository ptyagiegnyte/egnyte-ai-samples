# Egnyte MCP Server

An **MCP (Model Context Protocol)** server that connects to your Egnyte domain and exposes document search and retrieval functionality for use in **AI agents**.

This server uses **Egnyte’s public APIs and Python SDK** to support secure, real-time access to your Egnyte content, enabling smart AI-driven workflows while respecting existing permissions.

---
## ⚠️ Important: Choose the Right Solution

> **🌐 Remote MCP Server (Recommended)** - The **officially supported** solution by Egnyte
> - ✅ Best for most users and production use cases
> - ✅ Required for ChatGPT integration
> - ✅ OAuth authentication built-in
> - ✅ 200+ tools available out-of-the-box
> - ✅ No local setup or API wrapper development needed
> - 📖 **[Get Started with Remote MCP Server →](https://developers.egnyte.com/docs/Remote_MCP_Server)**
>
> **💻 Open-Source MCP Server (This Repo)** - For developers and experimentation
> - ⚡ Local development and testing only
> - ⚡ Requires API access token management
> - ⚡ Limited to basic search functionality
> - ⚡ Additional API wrappers must be implemented for extended functionality
> - ⚡ Best for developers who want to customize and extend the implementation

**For production use, ChatGPT/Claude integration, or if you need more than basic search functionality, use the [Remote MCP Server](#-remote-mcp-server-beta---for-chatgpt--oauth-compatible-clients).**

---


## 📑 Quick Navigation

- [About](#-about)
- [Tools Implemented](#️-tools-implemented)
- [Requirements](#-requirements)
- [Installation](#-installation)
- [Running the MCP Server](#-running-the-mcp-server)
- [Setting up MCP Clients](#-setting-up-mcp-clients)
  - [Cursor IDE Setup](#cursor-ide-setup)
  - [Claude Desktop Setup](#claude-desktop-setup)
- [**Remote MCP Server (Beta)** 🌐](#-remote-mcp-server-beta---for-chatgpt--oauth-compatible-clients)
  - [Quick Start Guide](#-getting-started)
  - [Which Server Should I Use?](#which-server-should-i-use)

---

### 📚 About

Model Context Protocol (MCP) is a framework to help AI agents securely query external systems for real-time context.  
The **Open Source Egnyte MCP Server** allows agents to:

- Search for documents by name
- Retrieve relevant documents from Egnyte
- Seamlessly integrate enterprise content into generative AI workflows

---

### 🛠️ Tools Implemented

| Tool Name                          | Description                                                                 |
|-----------------------------------|-----------------------------------------------------------------------------|
| `search_for_document_by_name`     | Searches for a document in your Egnyte domain using its filename.           |

---

### 📋 Requirements

- Python 3.11+
- Egnyte API access token - Register on https://developers.egnyte.com/member/register to get API key for your Egnyte account 
- An Egnyte domain with files to test


### Installing Prerequisites 

## 🔧 Installation

#### 1. Clone the Repository

```bash
git clone https://github.com/egnyte/egnyte-ai-samples.git
cd mcp-server
```

#### 2. Install `uv` (Python environment & dependency manager)

**Mac/Linux:**

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows (PowerShell):**

```powershell
irm https://astral.sh/uv/install.ps1 | iex
```

#### 3. Install Egnyte SDK

```bash
uv pip install egnyte
```

📄 [Egnyte SDK Documentation](https://developers.egnyte.com/egnyte_sdk#python)  
🔗 [Egnyte SDK GitHub](https://github.com/egnyte/python-egnyte)

#### 4. Setting up Environment Variables
1. **Create a `.env` File**

  Create a `.env` file inside the root directory with the following content:
  ```
  DOMAIN=your-egnyte-domain.egnyte.com 
  ACCESS_TOKEN=your-access-token-here
  ```
2. **Update with Your Credentials**

- Replace `your-egnyte-domain.egnyte.com` with your actual Egnyte domain.
- Replace `your-access-token-here` with your actual Egnyte API access token.

  This `.env` file is necessary for the server to authenticate and connect securely to your Egnyte domain.


### 🚀 Running the MCP Server

```bash
uv run server.py --python 3.11
```

This will start the MCP server locally and make the tools available to MCP-compliant clients.


#### ⚡ Setting up MCP Clients

This is a sample MCP (Model Context Protocol) client that connects to a locally running MCP server using `fastmcp`.  
It uses **Python Stdio Transport** to communicate with the server and call specific tools by name.

#### How it works

- Connects to the MCP server (`server.py`) via Python Stdio.
- Lists available tools exposed by the server.
- Calls a specific tool by its name, with provided arguments.
- The response is returned based on the tool execution.

#### Prerequisites

- Python 3.11+
- `fastmcp` library installed:
  ```bash
  uv pip install fastmcp
- MCP server (`server.py`) running locally
- `.env` file configured

#### Usage

1. **Ensure the MCP server is running** first.
2. **Run the client** using the command:
   ```bash
   python client.py

####  Cursor IDE Setup

1. Open Cursor → Settings → **MCP**
2. Click **"Add new global MCP server"**
3. Add the following configuration:

```json
{
  "mcpServers": {
    "Egnyte Document Retriever": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/egnyte-mcp-server",
        "run",
        "server.py"
      ]
    }
  }
}
```
✅ Replace `/path/to/egnyte-mcp-server` with your actual directory path.

4. Save and enable the server in the MCP settings.

---

### 🖼️ Example Screenshots

#### Cursor MCP Server Configuration

![Cursor MCP Config](images/cursor_mcp_config.png)

#### Cursor MCP Query in Action

![Cursor MCP Run](images/cursor_mcp_run.png)

---

####  Claude Desktop Setup

To connect Egnyte’s MCP server to Claude Desktop:

1. Launch Claude Desktop and open MCP tool configuration
2. Go to Settings → MCP Tools → Add New Server
3. Add the following configuration:

```json
{
  "mcpServers": {
    "egnyte": {
      "command": "python3",
      "args": ["server.py"],
      "cwd": "/Users/yourname/path/to/egnyte-mcp-server",
      "env": {
        "DOMAIN": "your-egnyte-domain.egnyte.com",
        "ACCESS_TOKEN": "your-access-token"
      }
    }
  }
}
```
✅ Replace ` /Users/yourname/path/to/egnyte-mcp-server ` with your actual directory path.

4. Save and Start - Claude should detect the server, list search_for_document_by_name under “Available MCP tools,” and be able to call it with filenames.

---
### 🖼️ Example Screenshots

#### Claude MCP Server Configuration

![Claude MCP Config](images/claude_mcp_config.png)

#### Claude MCP Query in Action

![Claude MCP Run](images/claude_mcp_run.png)

---
---

## 🌐 Remote MCP Server (Beta) - For ChatGPT & OAuth-Compatible Clients

## Open-Source vs. Remote Server

This repository contains the **open-source MCP server** for local, developer-focused implementations. It's perfect for:
- Local development with Cursor IDE and Claude Desktop
- Custom integrations where you manage authentication
- Self-hosted deployments

**Need ChatGPT or cloud-hosted integration?** We offer a fully-managed Remote MCP Server.

### Egnyte Remote MCP Server

The **Egnyte Remote MCP Server** provides enterprise-grade, cloud-hosted MCP integration with OAuth authentication - essential for ChatGPT and other OAuth-compatible clients.

**Server URL:** `https://mcp-server.egnyte.com/sse`

#### 🚀 Key Features
- **OAuth Authentication**: Enterprise-grade security without managing tokens
- **Extended Tool Set**: Beyond basic search - includes AI-powered document Q&A, summarization, workflow management, and more
- **Fully Managed**: No local setup or maintenance required
- **20+ Tools Available**: Including:
  - AI-powered document analysis (`ask_document`, `summarize_document`)
  - Egnyte Copilot integration (`ask_copilot`)
  - Knowledge Base queries (`ask_knowledge_base`)
  - Advanced search with metadata filtering
  - Workflow and project management tools


#### 📖 Getting Started

1. **Documentation**: Full setup guide at [https://developers.egnyte.com/docs/Remote_MCP_Server](https://developers.egnyte.com/docs/Remote_MCP_Server)

2. **Quick Setup for Claude**:
   - Go to Settings → Connectors → Add Custom Connector
   - Add URL: `https://mcp-server.egnyte.com/sse`
   - Authenticate with your Egnyte credentials
   - All 30+ tools become available immediately

3. **Quick Setup for ChatGPT** (Team/Enterprise/Edu workspaces):
   - Navigate to Custom Connectors in settings
   - Add URL: `https://mcp-server.egnyte.com/sse`
   - Authenticate with your Egnyte credentials
   - Note: Currently limited to search & fetch tools

#### 📋 Requirements
- **Beta Access**: Available now for testing for all plans, please reach out to account executive/CSM.
- **General Availability**: Ultimate plan required 
- **Compatible Clients**: Claude (all tools), ChatGPT (limited tools), any OAuth-compatible MCP client

#### 📧 Get Access
- **Express Interest**: [Fill out this form](https://forms.gle/ixMEZZRVAShV6VcY6)
- **Feedback**: Please report any issues on the remote server to pranav@egnyte.com

### Which Server Should I Use?

| Use Case | Recommended Server | Why |
|----------|-------------------|-----|
| Local development with Cursor | Open-Source (this repo) | Direct file access, no OAuth needed |
| ChatGPT integration | Remote MCP Server | OAuth required by ChatGPT |
| Claude Desktop (local files) | Open-Source (this repo) | Simpler setup for local development |
| Claude Pro/Team (cloud access) | Remote MCP Server | OAuth authentication, full tool access |
| Production/enterprise use | Remote MCP Server | Managed service, enterprise security |

---

#### 🔗 Helpful Links

If you'd like to fix something yourself, please fork this repository, commit the fixes and updates to tests, then set up a pull request with information what you're fixing.

- [Egnyte Developer Portal](https://developers.egnyte.com/)
- [Egnyte Python SDK](https://github.com/egnyte/python-egnyte)
- [Cursor MCP Documentation](https://docs.cursor.com/context/model-context-protocol)
