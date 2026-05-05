<div align="center">

![Power BI](https://img.shields.io/badge/PowerBI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![MCP](https://img.shields.io/badge/MCP-Server-blue?style=for-the-badge)
![AI Powered](https://img.shields.io/badge/AI-Powered-8A2BE2?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

**Develop Power BI models without ever opening Power BI Desktop.**

[What I Did](#-what-i-did) · [How to Setup](#-how-to-setup) · [Demo](#-demo) · [Limitations](#%EF%B8%8F-limitations) · [References](#-references)

</div>

---

## 📖 Table of Contents
- [What I Did](#-what-i-did)
- [Performance Agent](#-performance-agent)
- [How to Setup](#-how-to-setup)
- [Demo](#-demo)
- [Limitations](#%EF%B8%8F-limitations)
- [Files](#-files-in-this-repo)
- [References](#-references)

---

## 🚀 What I Did

As part of testing the PBI MCP Server, I used it to automate the data modelling process on a sample e-commerce dataset. Here's what I was able to produce through the LLM without touching Power BI Desktop:

| Feature | Description |
|--------|-------------|
| 🗂️ Star Schema | Split a flat file into dimension + fact tables with proper normalisation |
| 🔗 Relationships | Correctly established relationships across all tables |
| 🔢 Data Types | Assigned appropriate data types to every field (whole numbers, dates, decimals, percentages, and more) |
| 📐 DAX Measures | Generated commonly used measures based on the nature of the data |
| 📝 Descriptions | Wrote descriptions for every measure and column |

---

## 🤖 Performance Agent

I also built a simple agent (a plain text file) that analyses the data model and flags potential performance bottlenecks — **before you start building visuals.**

> 💡 This means you go into the visual-building phase already knowing what to expect, rather than discovering issues halfway through your report build.

---

## ⚙️ How to Setup

There are two ways to set up the PBI MCP Server: **automatically via Node.js** (using the `mcp.json` included in this repo) or **manually** if you don't have Node.js installed.

---

### Option 1 — Automatic (Node.js)

The `mcp.json` file in the `settings` folder is pre-configured to automatically download and run the MCP server via Node.js.

1. Make sure Node.js is installed on your machine
2. Copy the `mcp.json` from the `settings` folder into your IDE's MCP settings
3. The server will be downloaded and started automatically

---

### Option 2 — Manual Setup (No Node.js)

**Step 1 — Download the MCP Server**

Paste the following URL into your browser to download the PBI MCP Server from the Visual Studio Marketplace. Replace `<version>` with the latest version number (check the latest version [here](https://marketplace.visualstudio.com/items?itemName=analysis-services.powerbi-modeling-mcp)):

```
https://marketplace.visualstudio.com/_apis/public/gallery/publishers/analysis-services/vsextensions/powerbi-modeling-mcp/<version>/vspackage?targetPlatform=win32-x64
```

**Step 2 — Extract the File**

Unzip the downloaded file into a folder of your choice. Take note of the full folder path — you'll need it in the next step.

**Step 3 — Configure mcp.json**

In your IDE, locate the MCP settings file (typically under a `settings` folder as `mcp.json`) and add the following configuration. Replace the `command` value with the full path to `powerbi-modeling-mcp.exe` inside your extracted folder:

```json
{
  "mcpServers": {
    "powerbi-modeling-mcp": {
      "type": "stdio",
      "command": "<directory_where_powerbi-modeling-mcp.exe_file_located_in_local>",
      "args": [
        "--start"
      ],
      "env": {}
    }
  },
  "powers": {
    "mcpServers": {}
  }
}
```

---

> 📝 **IDE Notes**
>
> - The setup above was tested on **Kiro** (an agentic IDE by AWS). If you're using a different IDE that supports MCP, the same approach should work — just place the configuration in your IDE's `settings` folder under `mcp.json`.
> - If you're using **VS Code**, you can skip the manual download entirely and install the extension directly via the marketplace for a smoother setup. Follow the instructions in the [official repo](https://github.com/microsoft/powerbi-modeling-mcp).

---

## 🎬 Demo

**Source:** Sample e-commerce flat file — 1,000,000 rows · 62 columns

**Output produced in under 10 minutes:**

- ✅ 6 dimension tables + 1 fact table, properly normalised
- ✅ Relationships correctly set up across all tables
- ✅ All fields assigned the right data types
- ✅ 14 commonly used DAX measures, tailored to the data
- ✅ Descriptions added to every measure and column

The output file `PBIEcommercedone` is included in this repo for reference.

---

## ⚠️ Limitations

<details>
<summary>Click to expand</summary>

The MCP server currently does not support creating visuals through the LLM — that layer still requires Power BI Desktop.

However, there are additional tools that can be integrated to bring conversational AI into the visual-building process. This is actively being explored and will be updated here when ready.

</details>

---

## 📁 Files in This Repo

```
📦 pbi-mcp-server
 ┣ 📂 agents
 ┃ ┗ 📄 pbi-performance-tester.md     # Analyses your model and flags performance bottlenecks
 ┣ 📂 settings
 ┃ ┗ 📄 mcp.json                      # Auto-configures MCP server via Node.js
 ┣ 📂 steering
 ┃ ┗ 📄 pbi-bestpractices.md          # Power BI best practices guidance for the LLM
 ┗ 📄 PBIEcommercedone                # Output Power BI file from this demo
```

> 💡 **What is the `steering` folder?**
> If you're new to agentic IDEs, steering files might be an unfamiliar concept. Think of them as standing instructions you give to the LLM — they define the rules, guidelines, and best practices the AI should follow throughout your session. Without them, the LLM has no context on how you want it to behave. The `pbi-bestpractices.md` file in this repo tells the LLM to follow Power BI best practices when modelling your data, so you don't have to spell it out every time.

---

## 🔗 References

| Resource | Link |
|----------|------|
| Official PBI MCP Server | [microsoft/powerbi-modeling-mcp](https://github.com/microsoft/powerbi-modeling-mcp) |
| Sample E-commerce Dataset | [Global E-Commerce Dataset 1M Records — Kaggle](https://www.kaggle.com/datasets/akrambelha/global-e-commerce-dataset-1m-records-20242026) |
