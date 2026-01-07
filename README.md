# 🧪 Requirement Analysis Template

> **Transform Azure DevOps user stories into comprehensive test documentation with Claude AI**

This template project combines Python automation with Claude AI skills to create a complete requirements-based testing workflow. Clone it, configure your Azure DevOps connection, and start generating professional test cases from Gherkin user stories in minutes!

## ⚠️ Requirements

Before you begin, make sure you have:

- ✅ **Python 3.6+** installed on your machine
- ✅ **VS Code with Claude** (or Claude Desktop) for AI-powered test generation
- ✅ **Azure DevOps** account with user stories written in **Gherkin syntax** (Given/When/Then)
- ✅ **Personal Access Token (PAT)** from Azure DevOps with Work Items read permissions

## 🚀 What This Template Does

This is a ready-to-use project template that automates your entire test documentation workflow:

### 1️⃣ `export-user-story` Skill - Azure DevOps Export

Export acceptance criteria directly from Azure DevOps work items:

- Fetches user stories by ID via Azure DevOps REST API
- Converts HTML formatting to clean Gherkin syntax
- Preserves bullet points and proper formatting
- Saves to `docs/user-stories/` folder automatically

### 2️⃣ `testcase-creator` Skill - Test Documentation Generation

Transforms Gherkin scenarios into professional test documentation:

- **Requirement Analysis Document** - Detailed breakdown with scenario → requirement → test case mapping
- **Azure DevOps CSV Test Cases** - Import-ready test cases with proper formatting for Test Plans

### ✨ Key Features

- 🔄 **End-to-end automation** - From Azure DevOps to test cases in one command
- 🧠 **AI-powered analysis** - Claude intelligently combines related test cases
- 📊 **Complete traceability** - Hierarchical mapping from scenarios to requirements to test cases
- 📁 **Azure DevOps ready** - CSV files follow exact import format requirements
- 🎯 **BDD methodology** - Built on Behavior-Driven Development best practices
- 🔗 **Skill chaining** - Automatically chains export → analysis → test case generation

## 📁 Project Structure

```
requirement-analysis-template/
├── .claude/
│   └── skills/
│       ├── export-user-story/        # Azure DevOps export skill
│       ├── testcase-creator/         # Test case generation skill
│       └── skill-creator/            # Skill development framework
├── docs/
│   ├── user-stories/                 # Exported acceptance criteria (.us.txt)
│   ├── requirement-analysis/         # Generated requirement analysis documents
│   └── test-cases/                   # Generated Azure DevOps test cases (.csv)
├── src/
│   ├── .env                          # Azure DevOps credentials (not in git)
│   ├── .env.example                  # Template for .env file
│   └── export-acceptance-criteria.py # Python export script
├── requirements.txt                  # Python dependencies
├── prompt-example.md                 # Example usage prompts
└── README.md
```

## 🛠️ Quick Start Guide

### 📥 Step 1: Clone This Template

```bash
# Clone the repository to your machine
git clone https://github.com/yourusername/requirement-analysis-template.git

# Navigate into the folder
cd requirement-analysis-template
```

### 🐍 Step 2: Install Python (If Not Already Installed)

**Check if Python is installed:**

```bash
python --version
```

**If you see a version number (3.6 or higher), skip to Step 3!**

**If not, install Python:**

1. **Download Python**:

   - Go to [python.org/downloads](https://www.python.org/downloads/)
   - Download the latest version for your operating system

2. **Run the Installer**:

   - ⚠️ **IMPORTANT**: Check the box "Add Python to PATH" during installation
   - Click "Install Now"

3. **Verify Installation**:
   ```bash
   python --version
   ```
   - You should see something like `Python 3.11.0`

### 📦 Step 3: Install Python Dependencies

Open your terminal in the project folder and run:

```bash
# Install required packages
pip install -r requirements.txt
```

This installs:

- `requests` - For Azure DevOps API communication
- `python-dotenv` - For secure credential management

**💡 Tip**: If you get "pip not found", try `python -m pip install -r requirements.txt`

### 🔑 Step 4: Set Up Azure DevOps Connection

**Get your Personal Access Token (PAT):**

1. 🌐 Go to Azure DevOps in your browser
2. 👤 Click your profile picture (top right) → **Personal access tokens**
3. 🆕 Click **New Token**
4. 📝 Configure the token:
   - **Name**: `Requirement Analysis Tool`
   - **Expiration**: 90 days (or your preference)
   - **Scopes**: Select **Custom defined** and check:
     - ✅ **Work Items** → **Read**
5. 🔵 Click **Create**
6. 📋 **COPY THE TOKEN** (you won't see it again!)

**Configure your environment:**

1. Copy the example environment file:

   ```bash
   cp src/.env.example src/.env
   ```

2. Open `src/.env` and fill in your details:
   ```
   AZURE_DEVOPS_ORGANIZATION=your-organization-name
   AZURE_DEVOPS_PROJECT=your-project-name
   AZURE_DEVOPS_PAT=your-copied-token
   PRODUCT_PREFIX=eNr
   ```

**🔍 Where to find your information:**

- **Organization**: Your Azure DevOps URL → `https://dev.azure.com/YOUR-ORG`
- **Project**: The project name visible in Azure DevOps
- **PAT**: The token you just copied

### 🎯 Step 5: Run Your First Export

Now you're ready to use the skills! Open VS Code with Claude (or Claude Desktop) in this project folder.

**Example 1: Export and generate everything in one command**

```
@export-user-story 122019 --full
```

This will:

1. ✅ Export acceptance criteria from Azure DevOps
2. ✅ Generate requirement analysis document
3. ✅ Create Azure DevOps test case CSV

**Example 2: Generate test cases from existing file**

```
@testcase-creator generate test cases for eNr_122019_participants_tab.us.txt
```

See [prompt-example.md](prompt-example.md) for more examples!

### 📊 Step 6: Check Your Output

After running the commands, you'll find:

- 📄 **User Story** → `docs/user-stories/eNr_<id>_<title>.us.txt`
- 📋 **Analysis** → `docs/requirement-analysis/eNr_<id>_<title>.RequirementAnalysis.txt`
- 📊 **Test Cases** → `docs/test-cases/eNr_<id>_<title>.TestCases.csv`

**Import the CSV file directly into Azure DevOps Test Plans!**

## 💡 Usage Examples

See [prompt-example.md](prompt-example.md) for detailed examples.

**Example 1: Export and generate everything (Recommended)**

```
@export-user-story 122019 --full
```

**Example 2: Generate test cases from existing file**

```
@testcase-creator generate test cases for eNr_119222_workspace_-_tools_initial_setup.us.txt
```

## 🎯 Example Workflows

### Complete Workflow (Azure DevOps → Test Cases)

1. **Export from Azure DevOps:** `@export-user-story 122019 --full`
2. **Review outputs:**
   - Acceptance criteria in `docs/user-stories/`
   - Requirement analysis in `docs/requirement-analysis/`
   - CSV test cases in `docs/test-cases/`
3. **Import to Azure DevOps** - CSV files are ready for direct import

## 📖 Example Files

This repository includes complete examples:

- **[eNr_122019_participants_tab](docs/user-stories/eNr_122019_encounter_refresh-_user_directory_-_participants_tab.us.txt)** - Exported from Azure DevOps with bullet formatting
- **[eNr_121265_screen_share](docs/user-stories/eNr_121265_workspace-_screen_share.us.txt)** - 13-scenario screen sharing workflow

Each example includes generated requirement analysis and test case CSV files.

## 🎨 Test Case Design Philosophy

The skill follows a **smart combination strategy**:

- ✅ Combines related UI verifications into comprehensive test cases
- ✅ Keeps separate test cases for different user paths/workflows
- ✅ Validates error conditions independently from success paths
- ✅ Minimizes test case count while maintaining complete coverage

**Result:** Efficient, maintainable test suites that reduce execution time without sacrificing quality.

## 🧩 Skills Included

- **export-user-story** - Azure DevOps export with API integration
- **testcase-creator** - Test case generation from Gherkin scenarios
- **skill-creator** - Framework for creating and packaging skills

---

**Author** | Cody Huls | Jan. 7th 2026
