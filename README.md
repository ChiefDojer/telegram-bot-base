# Telegram Bot Base (Aiogram + Docker + Azure)

A production-ready Telegram bot template built with **Aiogram v3**, containerized with Docker, and optimized for deployment on **Azure Container Instances** or **Azure App Service**.

---

## Short Summary what need to be in instruction
- Create BOT in BotFather in Telegram and get BOT TOKEN
- Create a GitHub Repo with Telegram Bot Code (Python + Aiogram)
    - Add Project Structure
- Create Azure Cloud Ubunty VM and set up SSH for connection 
    - Install Docker to Azure Cloud Ubunty VM  
    - Download Bot Repo from GitHub to Azure Cloud Ubunty VM
    - Start Bot in the Docker Container
- Set Up CI for Automatic Bot Deploy to Azure Cloud Ubunty VM after chenges in Repo using GitHub Actions

## 📋 Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [1. Create Telegram Bot with BotFather](#1-create-telegram-bot-with-botfather)
  - [2. Clone Repository](#2-clone-repository)
  - [3. Configure Environment](#3-configure-environment)
  - [4. Local Development](#4-local-development)
- [Docker Setup](#docker-setup)
- [Testing](#testing)
- [CI/CD Pipeline](#cicd-pipeline)
- [Azure Deployment](#azure-deployment)
  - [Option A: Azure Ubuntu VM with Docker](#option-a-azure-ubuntu-vm-with-docker)
  - [Option B: Azure Container Instances (ACI)](#option-b-azure-container-instances-aci)
  - [Option C: Azure App Service](#option-c-azure-app-service)
- [Bot Commands](#bot-commands)
- [Customization](#customization)
- [Troubleshooting](#troubleshooting)

---

## ✨ Features

- **Aiogram v3**: Modern async Python framework for Telegram bots
- **Modular Architecture**: Separates handlers from main bot logic
- **Docker Support**: Containerized for consistent deployments with Docker Compose
- **Azure-Ready**: Pre-configured for Azure cloud hosting
- **Environment Variables**: Secure token management with `.env`
- **Long Polling**: Works out-of-the-box (webhook setup available)
- **Unit Testing**: Comprehensive test suite with pytest and coverage reporting
- **CI/CD Pipeline**: Automated testing and deployment with GitHub Actions
- **Timezone Support**: Date/time commands with timezone awareness (pytz)

---

## 📦 Prerequisites

Before you begin, ensure you have:

- **Python 3.11+** installed ([Download](https://www.python.org/downloads/))
- **Docker Desktop** (for containerization) ([Download](https://www.docker.com/products/docker-desktop/))
- **Azure CLI** (for Azure deployment) ([Install Guide](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli))
- **Azure Account** with an active subscription ([Free Trial](https://azure.microsoft.com/free/))
- **Git** installed ([Download](https://git-scm.com/downloads))

---

## 📁 Project Structure

```
telegram-bot-base/
├── .github/
│   └── workflows/
│       ├── test.yml         # CI workflow for running tests
│       └── deploy.yml       # CD workflow for Azure deployment
├── app/
│   ├── __init__.py          # Python package marker
│   ├── main.py              # Bot initialization & startup
│   ├── handlers.py          # Message/command handlers
│   ├── test_handlers.py     # Unit tests for handlers
│   ├── requirements.txt     # Python dependencies
│   └── requirements-test.txt # Testing dependencies
├── htmlcov/                 # HTML coverage reports (generated)
├── .env.example             # Environment variable template
├── .gitignore               # Git ignore patterns
├── pytest.ini               # Pytest configuration
├── docker-compose.yml       # Docker Compose configuration
├── Dockerfile               # Container build instructions
└── README.md                # This file
```

---

## 🚀 Getting Started

### 1. Create Telegram Bot with BotFather

**BotFather** is Telegram's official bot for creating and managing bots.

#### Steps:

1. Open Telegram and search for **@BotFather**
2. Start a chat and send `/newbot`
3. Follow the prompts:
   - **Bot Name**: Your bot's display name (e.g., "My Awesome Bot")
   - **Username**: Must end in `bot` (e.g., `my_awesome_bot`)
4. **Save the Token**: BotFather will provide an HTTP API token like:
   ```
   1234567890:ABCdefGHIjklMNOpqrsTUVwxyz123456789
   ```
   ⚠️ **Keep this token secret!** It's your bot's password.

#### Optional BotFather Commands:
- `/setdescription` - Set bot description
- `/setabouttext` - Set "About" section
- `/setcommands` - Register bot commands (autocomplete in Telegram)
- `/setuserpic` - Upload bot avatar

---

### 2. Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/telegram-bot-base.git
cd telegram-bot-base
```

---

### 3. Configure Environment

1. **Copy the example environment file:**
   ```bash
   cp .env.example .env
   ```

2. **Edit `.env` and add your bot token:**
   ```bash
   BOT_TOKEN=1234567890:ABCdefGHIjklMNOpqrsTUVwxyz123456789
   LOG_LEVEL=INFO
   ```

⚠️ **Never commit `.env` to Git!** (Already in `.gitignore`)

---

### 4. Local Development

#### Option A: Run with Python

1. **Create virtual environment:**
   ```bash
   python -m venv venv
   .\venv\Scripts\activate  # Windows PowerShell
   # source venv/bin/activate  # Linux/Mac
   ```

2. **Install dependencies:**
   ```bash
   pip install -r app/requirements.txt
   ```

3. **Run the bot:**
   ```bash
   python app/main.py
   ```

4. **Test in Telegram:**
   - Open your bot in Telegram
   - Send `/start`
   - You should receive a welcome message!

#### Option B: Run with Docker (Recommended)

```bash
# Build the image
docker build -t telegram-bot .

# Run the container
docker run --env-file .env telegram-bot
```

#### Option C: Run with Docker Compose

```bash
# Build and start the bot
docker-compose up -d

# View logs
docker-compose logs -f

# Stop the bot
docker-compose down
```

---

## 🐳 Docker Setup

### Dockerfile Explained

The included `Dockerfile` uses a multi-stage build for optimization:

- **Base Image**: `python:3.11-slim` (lightweight)
- **Working Directory**: `/app`
- **Dependencies**: Installed from `requirements.txt`
- **Entry Point**: Runs `main.py`

### Docker Compose Configuration

The `docker-compose.yml` provides a simplified deployment:

- **Service Name**: `telegram-bot` (used in docker compose commands)
- **Image Name**: `telegram-bot` (the Docker image name)
- **Container Name**: `telegram-bot-base` (the running container name)
- **Restart Policy**: `unless-stopped` (auto-restart on failure)
- **Environment**: Loaded from `.env` file
- **Logging**: JSON logging with rotation (max 10MB, 3 files)

### Docker Commands

```bash
# Build image
docker build -t telegram-bot:latest .

# Run locally
docker run --env-file .env telegram-bot:latest

# Run with custom token (override)
docker run -e BOT_TOKEN=your_token telegram-bot:latest

# Check logs
docker logs <container_id>

# Stop container
docker stop <container_id>
```

### Docker Compose Commands

```bash
# Start bot in background
docker-compose up -d

# View logs (live)
docker-compose logs -f telegram-bot

# Stop bot
docker-compose down

# Rebuild and restart
docker-compose up -d --build

# Check status
docker-compose ps
```

### Push to Docker Hub (Optional)

```bash
# Tag image
docker tag telegram-bot:latest YOUR_DOCKERHUB_USERNAME/telegram-bot:latest

# Login
docker login

# Push
docker push YOUR_DOCKERHUB_USERNAME/telegram-bot:latest
```

---

## 🧪 Testing

The project includes a comprehensive test suite using **pytest** with async support and coverage reporting.

### Running Tests

#### Install Test Dependencies

```bash
pip install -r app/requirements-test.txt
```

#### Run All Tests

```bash
# Run tests with coverage
pytest app/test_handlers.py --cov=handlers --cov-report=html

# Run tests without coverage
pytest app/test_handlers.py

# Run tests with verbose output
pytest app/test_handlers.py -v
```

#### View Coverage Report

After running tests with `--cov-report=html`, open the coverage report:

```bash
# Windows PowerShell
start htmlcov/index.html

# Linux/Mac
open htmlcov/index.html
```

### Test Structure

The test suite includes:

- **Unit Tests**: Tests for individual command handlers (`/start`, `/help`, `/about`, `/date`)
- **Message Handling Tests**: Tests for echo functionality and non-text messages
- **Integration Tests**: Tests for handler async behavior and parameter validation

### Writing New Tests

Example test for a new command:

```python
@pytest.mark.asyncio
async def test_new_command(self):
    """Test the new command"""
    message = create_mock_message(text="/newcommand")
    
    await new_command_handler(message)
    
    message.answer.assert_called_once()
    call_args = message.answer.call_args[0][0]
    assert "Expected text" in call_args
```

### Configuration

Tests are configured in `pytest.ini`:
- `pythonpath = app`: Sets the Python path to the app directory
- `asyncio_mode = auto`: Automatically detects async tests
- `asyncio_default_fixture_loop_scope = function`: Sets async fixture scope

---

## 🔄 CI/CD Pipeline

The project includes GitHub Actions workflows for automated testing and deployment.

### Workflows

#### 1. Unit Tests Workflow (`test.yml`)

Runs automatically on:
- **Push to `main` branch**
- **Pull requests to `main` branch**
- **Manual trigger** (workflow_dispatch)

**Steps:**
1. Checkout code
2. Set up Python 3.11
3. Install dependencies (requirements.txt + requirements-test.txt)
4. Run pytest with coverage reporting
5. Upload coverage reports to Codecov (optional)
6. Upload HTML coverage report as artifact
7. Comment coverage on pull requests

**Artifacts:**
- Coverage HTML reports (retained for 30 days)
- Coverage XML for Codecov integration

#### 2. Deploy Workflow (`deploy.yml`)

Runs automatically on:
- **Push to `main` branch** (after tests pass)
- **Manual trigger** (workflow_dispatch)

**Steps:**
1. Run tests (same as test.yml)
2. Deploy to Azure VM via SSH (only if tests pass)
   - Pull latest code from GitHub
   - Stop and remove only the `telegram-bot` service
   - Rebuild the `telegram-bot` service (no cache)
   - Start the `telegram-bot` container
   - Display container status and logs

**Dependencies:**
- Deploy job requires test job to succeed
- Tests must pass before deployment

**Important Notes:**
- The workflow targets only the `telegram-bot` service by name
- Other containers on the VM are not affected during deployment
- Uses service-specific commands to avoid disrupting other services
- Ensures clean rebuild with `--no-cache` flag

### Setting Up CI/CD

#### Required GitHub Secrets

For the deploy workflow to work, add these secrets to your repository:

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Add the following secrets:

| Secret Name | Description | Example |
|------------|-------------|---------|
| `SSH_HOST` | Azure VM public IP address | `20.123.456.789` |
| `SSH_USER` | SSH username for VM | `azureuser` |
| `SSH_PRIVATE_KEY` | Private SSH key for authentication | `-----BEGIN OPENSSH PRIVATE KEY-----...` |

#### Generating SSH Key for CI/CD

```bash
# On your local machine
ssh-keygen -t ed25519 -C "github-actions-bot" -f ~/.ssh/github_actions_key

# Display private key (add to SSH_PRIVATE_KEY secret)
cat ~/.ssh/github_actions_key

# Copy public key to Azure VM
ssh-copy-id -i ~/.ssh/github_actions_key.pub azureuser@YOUR_VM_IP

# Or manually add to VM's authorized_keys
cat ~/.ssh/github_actions_key.pub
# Then on VM: echo "PUBLIC_KEY_CONTENT" >> ~/.ssh/authorized_keys
```

### Viewing Test Results

#### In GitHub Actions UI

1. Navigate to **Actions** tab in your repository
2. Click on a workflow run
3. View test results and logs
4. Download coverage report artifacts

#### Coverage Reports

- **Terminal output**: Shows coverage % during workflow run
- **HTML report**: Download from workflow artifacts
- **Codecov** (optional): Set up Codecov account and add `CODECOV_TOKEN` secret for cloud coverage tracking

### Branch Protection Rules (Recommended)

Protect your `main` branch by requiring tests to pass:

1. Go to **Settings** → **Branches**
2. Add branch protection rule for `main`
3. Enable:
   - ✅ Require status checks to pass before merging
   - ✅ Require branches to be up to date
   - Select: `test` workflow
4. Save changes

This ensures no code can be merged to `main` without passing tests!

### Manual Workflow Triggers

Both workflows support manual triggering:

```bash
# Trigger from GitHub UI
1. Go to Actions tab
2. Select workflow (Test or Deploy)
3. Click "Run workflow"
4. Select branch
5. Click "Run workflow" button
```

### Workflow Status Badges

Add status badges to your README:

```markdown
![Tests](https://github.com/YOUR_USERNAME/telegram-bot-base/workflows/Run%20Unit%20Tests/badge.svg)
![Deploy](https://github.com/YOUR_USERNAME/telegram-bot-base/workflows/Deploy%20Telegram%20Bot%20to%20Azure%20VM/badge.svg)
```

---

## ☁️ Azure Deployment

### Prerequisites

1. **Install Azure CLI:**
   ```bash
   # Verify installation
   az --version
   ```

2. **Login to Azure:**
   ```bash
   az login
   ```

3. **Set subscription (if you have multiple):**
   ```bash
   az account set --subscription "YOUR_SUBSCRIPTION_ID"
   ```

---

### Option A: Azure Ubuntu VM with Docker

**Best for:** Full control over the environment, persistent VM hosting, cost-effective for long-running bots.

#### Step 1: Create Ubuntu VM

1. **Create VM via Azure Portal or CLI:**
   ```bash
   az vm create \
     --resource-group telegram-bot-rg \
     --name telegram-bot-vm \
     --image Ubuntu2204 \
     --size Standard_B1s \
     --admin-username azureuser \
     --generate-ssh-keys
   ```

2. **Connect to VM:**
   ```bash
   ssh azureuser@<VM_PUBLIC_IP>
   ```

#### Step 2: Install Docker on Ubuntu VM

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install Docker and dependencies
sudo apt install docker.io git python3-pip -y

# Verify Docker installation
docker --version
sudo systemctl status docker

# Enable Docker to start on boot
sudo systemctl enable docker
sudo systemctl start docker

# Add user to docker group (optional - avoid sudo)
sudo usermod -aG docker $USER
# Log out and back in for changes to take effect
```

#### Step 3: Clone Repository and Setup

```bash
# Clone your bot repository
git clone https://github.com/YOUR_USERNAME/telegram-bot-base.git
cd telegram-bot-base

# Create .env file with your bot token
echo "BOT_TOKEN=your_bot_token_here" > .env
echo "LOG_LEVEL=INFO" >> .env
```

#### Step 4: Build and Run with Docker Compose (Recommended)

```bash
# Build and start the bot using docker-compose
sudo docker compose up -d --build

# View logs
sudo docker compose logs -f telegram-bot

# Check status
sudo docker compose ps
```

**Alternative: Manual Docker Commands**

```bash
# Build Docker image
sudo docker build -t telegram-bot .

# Run container with auto-restart policy
sudo docker run -d \
  --name telegram-bot-base \
  --restart=unless-stopped \
  --env-file .env \
  telegram-bot
```

**Note:** Docker Compose automatically handles container naming, image tagging, and restart policies based on `docker-compose.yml` configuration.

#### Step 5: Verify Deployment

```bash
# Check container status
docker ps

# View live logs
docker logs -f telegram-bot

# Expected output:
# Bot started successfully!
# Start polling
# Run polling for bot ...
```

#### Step 6: Verify Container Configuration

Check the container details:

```bash
# Verify container name
docker ps --filter "name=telegram-bot-base"

# Verify restart policy
docker inspect -f '{{ .HostConfig.RestartPolicy.Name }}' telegram-bot-base
# Should output: unless-stopped

# Verify image name
docker inspect -f '{{ .Config.Image }}' telegram-bot-base
# Should output: telegram-bot
```

#### Step 7: Test Auto-Recovery

```bash
# Reboot VM to test auto-start
sudo reboot

# After reconnecting, verify bot is running
docker ps
# The container should show "Up ..."
```

#### Common Docker Commands for VM Deployment

**Using Docker Compose (Recommended):**

| Command | Description |
|---------|-------------|
| `sudo docker compose ps` | List containers managed by compose |
| `sudo docker compose logs -f telegram-bot` | View logs (live) for telegram-bot service |
| `sudo docker compose restart telegram-bot` | Restart only telegram-bot service |
| `sudo docker compose stop telegram-bot` | Stop telegram-bot service |
| `sudo docker compose start telegram-bot` | Start telegram-bot service |
| `sudo docker compose down` | Stop and remove all compose containers |
| `sudo docker compose up -d --build telegram-bot` | Rebuild and restart telegram-bot only |

**Using Docker Directly:**

| Command | Description |
|---------|-------------|
| `docker ps -a` | List all containers (running and stopped) |
| `docker logs -f telegram-bot-base` | View container logs (live) |
| `docker restart telegram-bot-base` | Restart the bot container |
| `docker stop telegram-bot-base` | Stop the bot |
| `docker start telegram-bot-base` | Start stopped container |
| `docker rm -f telegram-bot-base` | Remove container (force) |
| `docker images` | List all images (look for telegram-bot) |

#### Updating the Bot

**Recommended Method (Docker Compose):**

```bash
# Navigate to project directory
cd telegram-bot-base

# Pull latest changes from GitHub
git pull origin main

# Stop and remove only the telegram-bot service
sudo docker compose stop telegram-bot
sudo docker compose rm -f telegram-bot

# Rebuild and start telegram-bot service
sudo docker compose up -d --build telegram-bot

# View logs to verify successful restart
sudo docker compose logs --tail=50 telegram-bot
```

**Alternative Method (Manual Docker):**

```bash
# Navigate to project directory
cd telegram-bot-base

# Pull latest changes from GitHub
git pull origin main

# Stop and remove old container
docker stop telegram-bot-base
docker rm telegram-bot-base

# Rebuild image with new code
docker build -t telegram-bot .

# Run updated container
docker run -d \
  --name telegram-bot-base \
  --restart=unless-stopped \
  --env-file .env \
  telegram-bot
```

**Note:** The CI/CD workflow automatically follows the Docker Compose method when you push to the main branch.

#### Security Recommendations for VM Deployment

1. **Configure Network Security Group (NSG):**
   - Only allow SSH (port 22) from your IP
   - No need to expose other ports for long-polling bots

2. **Secure SSH access:**
   ```bash
   # Disable password authentication (use keys only)
   sudo nano /etc/ssh/sshd_config
   # Set: PasswordAuthentication no
   sudo systemctl restart ssh
   ```

3. **Keep system updated:**
   ```bash
   # Set up automatic security updates
   sudo apt install unattended-upgrades -y
   sudo dpkg-reconfigure --priority=low unattended-upgrades
   ```

4. **Monitor disk space:**
   ```bash
   # Check disk usage
   df -h
   
   # Clean Docker resources periodically
   docker system prune -a
   ```

---

### Option B: Azure Container Instances (ACI)

**Best for:** Simple, serverless container hosting with pay-per-second billing.

#### Step 1: Create Resource Group

```bash
az group create --name telegram-bot-rg --location eastus
```

#### Step 2: Create Azure Container Registry (ACR)

```bash
# Create registry
az acr create --resource-group telegram-bot-rg \
  --name yourbotregistry --sku Basic

# Login to ACR
az acr login --name yourbotregistry
```

#### Step 3: Build & Push Image to ACR

```bash
# Tag image for ACR
docker tag telegram-bot:latest yourbotregistry.azurecr.io/telegram-bot:latest

# Push to ACR
docker push yourbotregistry.azurecr.io/telegram-bot:latest
```

#### Step 4: Deploy to ACI

```bash
az container create \
  --resource-group telegram-bot-rg \
  --name telegram-bot-container \
  --image yourbotregistry.azurecr.io/telegram-bot:latest \
  --cpu 1 --memory 1 \
  --registry-login-server yourbotregistry.azurecr.io \
  --registry-username yourbotregistry \
  --registry-password $(az acr credential show --name yourbotregistry --query "passwords[0].value" -o tsv) \
  --environment-variables BOT_TOKEN=your_bot_token_here LOG_LEVEL=INFO \
  --restart-policy Always
```

#### Step 5: Verify Deployment

```bash
# Check status
az container show --resource-group telegram-bot-rg \
  --name telegram-bot-container --query instanceView.state

# View logs
az container logs --resource-group telegram-bot-rg \
  --name telegram-bot-container
```

---

### Option C: Azure App Service

**Best for:** Web app hosting with built-in CI/CD and scaling.

#### Step 1: Create App Service Plan

```bash
az appservice plan create \
  --name telegram-bot-plan \
  --resource-group telegram-bot-rg \
  --is-linux --sku B1
```

#### Step 2: Create Web App with Container

```bash
az webapp create \
  --resource-group telegram-bot-rg \
  --plan telegram-bot-plan \
  --name your-telegram-bot-app \
  --deployment-container-image-name yourbotregistry.azurecr.io/telegram-bot:latest
```

#### Step 3: Configure Environment Variables

```bash
az webapp config appsettings set \
  --resource-group telegram-bot-rg \
  --name your-telegram-bot-app \
  --settings BOT_TOKEN=your_bot_token_here LOG_LEVEL=INFO
```

#### Step 4: Enable Container Logging

```bash
az webapp log config \
  --resource-group telegram-bot-rg \
  --name your-telegram-bot-app \
  --docker-container-logging filesystem
```

#### Step 5: Restart & Monitor

```bash
# Restart app
az webapp restart --resource-group telegram-bot-rg \
  --name your-telegram-bot-app

# Stream logs
az webapp log tail --resource-group telegram-bot-rg \
  --name your-telegram-bot-app
```

---

## 🤖 Bot Commands

The template includes these default commands (defined in `handlers.py`):

| Command | Description |
|---------|-------------|
| `/start` | Welcome message and bot introduction |
| `/help` | Display available commands |
| `/about` | Show bot information |
| `/date` | Show current date and time with timezone |

### Registering Commands with BotFather

To enable autocomplete in Telegram:

1. Message **@BotFather**
2. Send `/setcommands`
3. Select your bot
4. Paste this list:
   ```
   start - Start the bot
   help - Show help message
   about - About this bot
   date - Show current date and time
   ```

---

## 🎨 Customization

### Adding New Handlers

Edit `app/handlers.py`:

```python
from aiogram import Router, F
from aiogram.types import Message
from aiogram.filters import Command

router = Router()

@router.message(Command("custom"))
async def custom_command(message: Message):
    await message.answer("This is a custom command!")

@router.message(F.text.contains("hello"))
async def hello_trigger(message: Message):
    await message.reply("Hello! 👋")
```

### Using Inline Keyboards

```python
from aiogram.types import InlineKeyboardMarkup, InlineKeyboardButton

@router.message(Command("menu"))
async def show_menu(message: Message):
    keyboard = InlineKeyboardMarkup(inline_keyboard=[
        [InlineKeyboardButton(text="Option 1", callback_data="opt1")],
        [InlineKeyboardButton(text="Option 2", callback_data="opt2")]
    ])
    await message.answer("Choose an option:", reply_markup=keyboard)
```

### Environment Variables

Add new variables to `.env`:
```bash
DATABASE_URL=postgresql://user:pass@host/db
API_KEY=your_api_key
```

Access in code:
```python
import os
db_url = os.getenv("DATABASE_URL")
```

---

## 🔧 Troubleshooting

### Bot Not Responding

1. **Check token validity:**
   ```bash
   curl https://api.telegram.org/bot<YOUR_TOKEN>/getMe
   ```

2. **Verify bot is running:**
   ```bash
   # Local
   ps aux | grep python
   
   # Docker
   docker ps
   
   # Azure ACI
   az container show --resource-group telegram-bot-rg --name telegram-bot-container
   ```

3. **Check logs:**
   ```bash
   # Docker
   docker logs <container_id>
   
   # Azure
   az container logs --resource-group telegram-bot-rg --name telegram-bot-container
   ```

### Common Errors

**"Conflict: terminated by other getUpdates request"**
- Your bot is running in multiple places. Stop all instances except one.

**"Unauthorized"**
- Invalid bot token. Double-check `.env` or Azure environment variables.

**Azure deployment fails**
- Verify ACR credentials: `az acr credential show --name yourbotregistry`
- Check resource quotas: `az vm list-usage --location eastus`

### Rate Limits

Telegram limits:
- **30 messages/second** per bot
- **20 messages/minute** per group

Use Aiogram's built-in rate limiting:
```python
from aiogram import Dispatcher
from aiogram.fsm.storage.memory import MemoryStorage

dp = Dispatcher(storage=MemoryStorage())
```

---

## 📚 Additional Resources

- [Aiogram Documentation](https://docs.aiogram.dev/)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Azure Container Instances Docs](https://learn.microsoft.com/en-us/azure/container-instances/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Pytest Documentation](https://docs.pytest.org/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

---

## 🆕 Recent Updates

### CI/CD Pipeline (November 2025)
- **Automated Testing**: GitHub Actions workflow runs unit tests on every push to `main` and pull requests
- **Continuous Deployment**: Automatic deployment to Azure VM after tests pass
- **Test-First Deployment**: Deploy workflow only runs if all tests succeed
- **Coverage Reports**: Automated coverage report generation and artifact uploads
- **Branch Protection**: Support for requiring passing tests before merging

### Testing Infrastructure (November 2025)
- Added comprehensive unit tests with `pytest`
- Integrated `pytest-asyncio` for testing async handlers
- Added code coverage reporting with `pytest-cov`
- Created `pytest.ini` configuration for async test support
- HTML coverage reports generated in `htmlcov/` directory

### New Features
- **`/date` command**: Shows current date and time with timezone support
- **Timezone support**: Added `pytz` dependency for accurate timezone handling
- **Docker Compose**: Simplified deployment with `docker-compose.yml`
- **Git ignore**: Added comprehensive `.gitignore` for Python projects

### Improvements
- Enhanced handler tests covering all command scenarios
- Mock utilities for testing Telegram message objects
- Logging configuration with configurable log levels
- Automatic test discovery with proper Python path configuration

---

## 📝 License

MIT License - feel free to use for any project.

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

**Need help?** Open an issue on GitHub or check the Aiogram community chat.
