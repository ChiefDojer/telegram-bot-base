# PRD: Telegram Bot Base Template

## 1. Product overview

### 1.1 Document title and version

* PRD: Telegram Bot Base Template
* Version: 1.0
* Last Updated: November 13, 2025

### 1.2 Product summary

The Telegram Bot Base Template is a production-ready, open-source framework designed to accelerate the development and deployment of Telegram bots. Built on Aiogram v3, the project provides a robust foundation with modern Python async patterns, containerized deployment, comprehensive testing infrastructure, and automated CI/CD pipelines.

The template serves as a starting point for developers who want to quickly build and deploy Telegram bots to Azure cloud infrastructure without reinventing the wheel. It includes best practices for code organization, testing, deployment automation, and monitoring, making it suitable for both learning purposes and production use cases.

The current implementation includes a basic echo bot with multiple command handlers, demonstrating the architecture and patterns that can be extended for more complex bot functionality.

## 2. Goals

### 2.1 Business goals

* Reduce time-to-market for Telegram bot development from weeks to hours
* Provide a maintainable, scalable foundation for bot projects
* Enable continuous deployment with automated testing to minimize downtime
* Establish best practices for Python-based Telegram bot development
* Support Azure cloud deployment with cost-effective hosting options
* Create an educational resource for developers learning bot development

### 2.2 User goals

* Quickly bootstrap a new Telegram bot project with minimal configuration
* Deploy bots to production with confidence through automated testing
* Understand bot development patterns through clear, documented code
* Extend the template with custom functionality without breaking existing features
* Monitor and debug bot behavior through comprehensive logging
* Maintain code quality through built-in testing infrastructure

### 2.3 Non-goals

* This is not a feature-rich bot application - it's a template/framework
* Does not include database integration (to keep the template simple)
* Does not provide a web dashboard or admin interface
* Does not include webhook-based deployment (focuses on long polling)
* Does not support multi-language internationalization out of the box
* Does not include payment processing or advanced bot features

## 3. User personas

### 3.1 Key user types

* Python developers building Telegram bots
* DevOps engineers deploying bot infrastructure
* Students learning bot development
* Open-source contributors

### 3.2 Basic persona details

* **Bot Developer (Primary)**: A Python developer with intermediate skills who wants to build a Telegram bot quickly. They understand async/await patterns, have basic Docker knowledge, and want a proven deployment pipeline. They value clean code, good documentation, and automated testing.

* **DevOps Engineer (Secondary)**: Responsible for deploying and maintaining bot infrastructure. They need containerized applications, CI/CD automation, and clear deployment instructions. They prefer infrastructure-as-code and want monitoring capabilities.

* **Learning Developer (Tertiary)**: A student or junior developer learning Telegram bot development. They need clear examples, well-commented code, and comprehensive documentation to understand bot architecture and best practices.

### 3.3 Role-based access

* **Repository Owner**: Full access to GitHub repository, can merge PRs, manage secrets, deploy to production
* **Contributor**: Can fork repository, submit pull requests, run tests locally
* **Bot Administrator**: Has the Telegram bot token, can configure environment variables, monitor bot behavior
* **End User**: Telegram users who interact with deployed bots built from this template

## 4. Functional requirements

* **Bot Framework Integration** (Priority: Critical)
  * Must use Aiogram v3 for Telegram Bot API interaction
  * Must support async/await patterns throughout the codebase
  * Must handle long polling for message updates
  * Must support HTML parse mode for formatted messages

* **Command Handling** (Priority: Critical)
  * Must implement /start command for bot initialization
  * Must implement /help command showing available commands
  * Must implement /about command showing bot information
  * Must implement /date command showing current time with timezone
  * Must handle text message echoing for demonstration
  * Must handle non-text messages with appropriate responses

* **Environment Configuration** (Priority: Critical)
  * Must load bot token from environment variables
  * Must support configurable log levels (DEBUG, INFO, WARNING, ERROR)
  * Must never commit sensitive credentials to version control
  * Must provide .env.example template for configuration

* **Containerization** (Priority: High)
  * Must provide Dockerfile for container builds
  * Must include docker-compose.yml for simplified deployment
  * Must use Python 3.11 slim base image for efficiency
  * Must support automatic container restart on failure
  * Must implement log rotation (10MB max, 3 files)

* **Testing Infrastructure** (Priority: High)
  * Must include comprehensive unit tests for all handlers
  * Must achieve 80%+ code coverage
  * Must use pytest with async support
  * Must provide mock utilities for Telegram objects
  * Must generate HTML coverage reports
  * Must support local test execution

* **CI/CD Pipeline** (Priority: High)
  * Must run automated tests on every push to main branch
  * Must run automated tests on all pull requests
  * Must block deployment if tests fail
  * Must deploy to Azure VM after successful tests
  * Must support manual workflow triggers
  * Must upload coverage artifacts for review

* **Logging and Monitoring** (Priority: Medium)
  * Must log all bot startup events
  * Must log errors with stack traces
  * Must support configurable log formatting
  * Must include timestamp and log level in output
  * Must log deployment events in CI/CD pipeline

* **Documentation** (Priority: High)
  * Must provide comprehensive README with setup instructions
  * Must document all commands and their purpose
  * Must include Azure deployment guides (VM, ACI, App Service)
  * Must document testing procedures
  * Must include CI/CD setup instructions
  * Must provide troubleshooting section

## 5. User experience

### 5.1 Entry points & first-time user flow

* Developer discovers the template on GitHub
* Developer reads README to understand capabilities
* Developer clones repository to local machine
* Developer creates bot via @BotFather in Telegram
* Developer configures .env file with bot token
* Developer runs bot locally to test basic functionality
* Developer commits changes and sets up CI/CD
* Developer deploys to Azure cloud infrastructure

### 5.2 Core experience

* **Local Development**: Developers can run the bot locally with minimal setup. Virtual environment installation and running the bot should take less than 5 minutes. The bot responds immediately to commands, and logs provide clear feedback about bot behavior.

* **Testing**: Developers can run tests with a single command. Test output is verbose and clear, showing which handlers are tested. Coverage reports are generated automatically and are easy to understand.

* **Deployment**: Push to main triggers automatic testing and deployment. Developers receive immediate feedback via GitHub Actions. The bot automatically restarts on the Azure VM with zero manual intervention.

* **Extension**: Developers can add new commands by following the existing handler patterns. The modular architecture makes it easy to understand where to add code. Tests can be written following the existing test patterns.

### 5.3 Advanced features & edge cases

* Manual workflow triggers for testing deployment without pushing code
* Docker Compose for local multi-container development
* Branch protection rules to enforce code quality
* Coverage reporting with Codecov integration
* PR comments with coverage changes
* Support for multiple deployment targets (VM, ACI, App Service)

### 5.4 UI/UX highlights

* **Bot Interaction**: Clean, emoji-enhanced responses for better user engagement
* **Error Messages**: User-friendly error messages when bot receives non-text content
* **Help System**: Comprehensive command listing accessible via /help
* **HTML Formatting**: Bold text and structured formatting for readability
* **Greeting**: Personalized welcome messages using user's first name

## 6. Narrative

A developer wants to build a Telegram bot for their community but doesn't want to spend weeks setting up infrastructure. They discover the Telegram Bot Base Template and clone it. Within 10 minutes, they have a bot running locally. They add their custom commands following the clear handler patterns, write tests mimicking the existing test structure, and commit their code.

The CI/CD pipeline automatically runs tests and deploys to their Azure VM. They can focus entirely on building features rather than wrestling with deployment pipelines, Docker configurations, or test infrastructure. When they want to add a new feature, they create a pull request and GitHub Actions shows test results and coverage changes. Their bot scales from a simple echo bot to a feature-rich application while maintaining code quality through automated testing.

## 7. Success metrics

### 7.1 User-centric metrics

* Time from clone to first local bot run: < 10 minutes
* Time from repository setup to production deployment: < 1 hour
* Test coverage maintained above 80%
* Developer satisfaction score (based on GitHub stars/issues)
* Number of successful forks and derivatives
* Community contributions (pull requests, issues, discussions)

### 7.2 Business metrics

* Reduction in bot development time: 70% compared to starting from scratch
* Zero-downtime deployments through automated CI/CD
* Cost efficiency: Running on $5-15/month Azure VM instances
* Code quality: All PRs must pass tests before merge
* Documentation completeness: 100% of features documented

### 7.3 Technical metrics

* Test execution time: < 30 seconds
* Docker build time: < 2 minutes
* Deployment time: < 3 minutes from push to live
* Container restart time: < 10 seconds
* Bot response time: < 100ms for simple commands
* CI/CD pipeline success rate: > 95%

## 8. Technical considerations

### 8.1 Integration points

* **Telegram Bot API**: Primary integration for receiving and sending messages
* **GitHub Actions**: CI/CD automation platform
* **Azure VM**: Primary deployment target via SSH
* **Docker Hub**: Optional for sharing container images
* **Codecov**: Optional for coverage tracking
* **Git/GitHub**: Version control and collaboration
* **PyPI**: Python package dependencies

### 8.2 Data storage & privacy

* No database required for the template
* Bot token stored in environment variables (not committed)
* SSH keys stored in GitHub Secrets
* User messages not persisted (ephemeral processing)
* Logs contain user messages but are rotated regularly
* No personally identifiable information stored long-term
* Compliant with Telegram's Bot API terms of service

### 8.3 Scalability & performance

* Long polling suitable for < 1000 users
* Async architecture supports concurrent message handling
* Docker allows horizontal scaling by running multiple instances
* Stateless design enables easy scaling
* Log rotation prevents disk space issues
* Automatic container restarts ensure high availability
* Can migrate to webhook-based deployment for higher scale

### 8.4 Potential challenges

* **Rate Limiting**: Telegram enforces 30 messages/second limit - template doesn't implement rate limiting
* **Long Polling vs Webhooks**: Long polling less efficient for high-traffic bots
* **State Management**: Template is stateless - complex conversations require state management
* **Error Recovery**: Bot restarts on crash but loses in-flight message context
* **Testing Limitations**: Mock-based testing may not catch Telegram API changes
* **Azure Costs**: VM costs accumulate monthly, need cost monitoring
* **SSH Key Management**: Lost keys require manual VM access recovery

## 9. Milestones & sequencing

### 9.1 Project estimate

* Small: This is a template/framework project, not a feature development project
* Estimated time for new features: 1-2 weeks per major enhancement
* Estimated time for developers to adopt: 1-2 hours

### 9.2 Team size & composition

* Ideal team size: 1-2 developers
* Roles: Backend Developer (Python/Telegram Bot expertise), DevOps Engineer (Azure/Docker expertise)
* For template maintenance: 1 developer part-time

### 9.3 Suggested phases

* **Phase 1: Template Improvements** (1-2 weeks)
  * Add database integration example
  * Add conversation state management example
  * Add webhook deployment option
  * Implement rate limiting middleware

* **Phase 2: Enhanced Testing** (1 week)
  * Add integration tests with Telegram test environment
  * Add load testing framework
  * Add performance benchmarking
  * Expand test coverage to 95%+

* **Phase 3: Advanced Features** (2-3 weeks)
  * Add inline keyboard examples
  * Add file upload/download handlers
  * Add multi-language support
  * Add scheduled task examples

* **Phase 4: DevOps Enhancements** (1-2 weeks)
  * Add Terraform templates for Azure infrastructure
  * Add monitoring with Prometheus/Grafana
  * Add automated backup strategies
  * Add blue-green deployment option

## 10. User stories

### 10.1. Bot initialization

* **ID**: GH-001
* **Description**: As a Telegram user, I want to start the bot with /start command so that I receive a welcome message and know the bot is active.
* **Acceptance criteria**:
  * When I send /start, the bot responds within 1 second
  * The response includes my first name in a personalized greeting
  * The response mentions that the bot is built with Aiogram v3
  * The response directs me to use /help for more commands
  * The response uses HTML formatting with bold text
  * The response includes an emoji (👋) for visual appeal

### 10.2. View available commands

* **ID**: GH-002
* **Description**: As a Telegram user, I want to view all available commands via /help so that I know what the bot can do.
* **Acceptance criteria**:
  * When I send /help, the bot lists all available commands
  * Each command includes a brief description
  * The list includes /start, /help, /about, and /date commands
  * The response mentions echo functionality
  * The response uses HTML formatting with headers
  * The response includes an emoji (📚) in the header

### 10.3. Learn about the bot

* **ID**: GH-003
* **Description**: As a Telegram user, I want to see information about the bot via /about so that I understand its purpose and technical stack.
* **Acceptance criteria**:
  * When I send /about, the bot provides technical information
  * The response mentions Aiogram v3 framework
  * The response mentions Python 3.11+ language
  * The response mentions Azure hosting
  * The response includes attribution to the template
  * The response uses HTML formatting with headers and emojis

### 10.4. Check current date and time

* **ID**: GH-004
* **Description**: As a Telegram user, I want to get the current date and time via /date so that I can see timezone-aware time information.
* **Acceptance criteria**:
  * When I send /date, the bot responds with current date/time
  * The response includes timezone information (UTC)
  * The date format is YYYY-MM-DD HH:MM:SS TZ
  * The response includes an emoji (📅)
  * The time is accurate within 1 second
  * The timezone can be configured in the code

### 10.5. Echo text messages

* **ID**: GH-005
* **Description**: As a Telegram user, I want to send text messages and receive them echoed back so that I can test the bot's responsiveness.
* **Acceptance criteria**:
  * When I send any text message (not a command), the bot echoes it back
  * The response format is "You said: [my message]"
  * Special characters are preserved in the echo
  * Empty messages are handled without error
  * The bot responds within 1 second
  * Long messages (up to 4096 characters) are handled

### 10.6. Handle non-text messages

* **ID**: GH-006
* **Description**: As a Telegram user, I want to receive a helpful message when I send non-text content so that I understand the bot's limitations.
* **Acceptance criteria**:
  * When I send a photo, the bot responds with a limitation message
  * When I send a sticker, the bot responds with a limitation message
  * When I send a video, the bot responds with a limitation message
  * When I send a document, the bot responds with a limitation message
  * The message explains "I can only handle text messages for now"
  * The response includes an emoji (📝)

### 10.7. Run bot locally for development

* **ID**: GH-007
* **Description**: As a developer, I want to run the bot on my local machine so that I can test changes before deploying to production.
* **Acceptance criteria**:
  * I can clone the repository from GitHub
  * I can create a .env file with my bot token
  * I can install dependencies with pip from requirements.txt
  * I can run the bot with `python app/main.py`
  * The bot connects to Telegram and starts polling
  * I can test all commands in my Telegram client
  * The bot logs startup messages to console

### 10.8. Run automated tests

* **ID**: GH-008
* **Description**: As a developer, I want to run automated tests locally so that I can verify my changes don't break existing functionality.
* **Acceptance criteria**:
  * I can install test dependencies from requirements-test.txt
  * I can run tests with `pytest app/test_handlers.py`
  * All tests pass on a clean checkout
  * Test results show which handlers were tested
  * Coverage report is generated showing >80% coverage
  * I can view HTML coverage report in a browser
  * Tests complete in under 30 seconds

### 10.9. Deploy bot with Docker

* **ID**: GH-009
* **Description**: As a developer, I want to deploy the bot using Docker so that I have a consistent runtime environment.
* **Acceptance criteria**:
  * I can build a Docker image with `docker build`
  * I can run the container with `docker run --env-file .env`
  * I can use docker-compose for simplified deployment
  * The container restarts automatically on failure
  * Logs are rotated to prevent disk space issues
  * I can view logs with `docker logs` command
  * Environment variables are loaded from .env file

### 10.10. Automatic deployment via CI/CD

* **ID**: GH-010
* **Description**: As a developer, I want my bot to deploy automatically when I push to main so that I don't have to manually deploy changes.
* **Acceptance criteria**:
  * Pushing to main triggers GitHub Actions workflow
  * Tests run automatically before deployment
  * If tests fail, deployment is blocked
  * If tests pass, bot deploys to Azure VM via SSH
  * Old container is stopped and removed
  * New container is built and started
  * I receive notification of deployment success/failure
  * Deployment completes within 5 minutes of push

### 10.11. View test results in CI

* **ID**: GH-011
* **Description**: As a developer, I want to see test results in GitHub Actions so that I know if my changes broke anything.
* **Acceptance criteria**:
  * Test workflow runs on every push to main
  * Test workflow runs on every pull request
  * I can view test results in the GitHub Actions UI
  * Failed tests show clear error messages
  * Coverage report is uploaded as an artifact
  * Coverage changes are commented on pull requests
  * I can manually trigger test workflow

### 10.12. Configure bot environment

* **ID**: GH-012
* **Description**: As a developer, I want to configure the bot via environment variables so that I can use different settings for development and production.
* **Acceptance criteria**:
  * BOT_TOKEN is required and loaded from environment
  * LOG_LEVEL is optional and defaults to INFO
  * If BOT_TOKEN is missing, bot logs error and exits
  * I can use .env file for local development
  * I can use Azure VM environment for production
  * Sensitive values are never committed to Git
  * .env.example provides template for configuration

### 10.13. Monitor bot logs

* **ID**: GH-013
* **Description**: As a developer, I want to view bot logs so that I can debug issues and monitor behavior.
* **Acceptance criteria**:
  * Logs include timestamp for each entry
  * Logs include log level (INFO, ERROR, etc.)
  * Logs include logger name (module name)
  * Startup events are logged at INFO level
  * Errors include full stack traces
  * Logs are viewable with `docker logs -f`
  * Logs are rotated at 10MB (3 files kept)

### 10.14. Add new bot commands

* **ID**: GH-014
* **Description**: As a developer, I want to add new commands to the bot so that I can extend its functionality.
* **Acceptance criteria**:
  * I can create new handler functions in handlers.py
  * I can use @router.message(Command("mycommand")) decorator
  * New handlers follow the async def pattern
  * I can access message content and user information
  * I can send responses with message.answer()
  * New commands appear when I rebuild and restart
  * Existing commands continue to work

### 10.15. Write tests for new features

* **ID**: GH-015
* **Description**: As a developer, I want to write tests for new commands so that I maintain code quality and prevent regressions.
* **Acceptance criteria**:
  * I can use the create_mock_message() helper function
  * I can write async test functions with @pytest.mark.asyncio
  * I can assert on message.answer call arguments
  * I can test multiple scenarios (success, edge cases)
  * Tests run with the rest of the test suite
  * Coverage report includes my new tests
  * I can follow existing test patterns

### 10.16. Set up Azure VM deployment

* **ID**: GH-016
* **Description**: As a DevOps engineer, I want to set up Azure VM for bot deployment so that the bot runs 24/7 in the cloud.
* **Acceptance criteria**:
  * I can create Ubuntu VM via Azure Portal or CLI
  * I can install Docker on the VM
  * I can clone the bot repository to the VM
  * I can configure .env file on the VM
  * I can build and run Docker container on the VM
  * Bot starts automatically on VM reboot
  * I can access the VM via SSH
  * I can view logs via SSH

### 10.17. Configure GitHub Actions secrets

* **ID**: GH-017
* **Description**: As a DevOps engineer, I want to configure GitHub secrets for deployment so that CI/CD can deploy to Azure VM securely.
* **Acceptance criteria**:
  * I can add SSH_HOST secret with VM IP address
  * I can add SSH_USER secret with SSH username
  * I can add SSH_PRIVATE_KEY secret with private key
  * Secrets are encrypted by GitHub
  * Secrets are not visible in workflow logs
  * Deploy workflow uses secrets for SSH connection
  * I can update secrets without changing workflows

### 10.18. Fork and customize template

* **ID**: GH-018
* **Description**: As a developer, I want to fork the template repository so that I can create my own bot based on this foundation.
* **Acceptance criteria**:
  * I can fork the repository on GitHub
  * I can clone my fork to my local machine
  * I can customize bot name and responses
  * I can add my own commands and handlers
  * I can push changes to my fork
  * I can set up my own CI/CD with my Azure VM
  * Original template attribution is preserved

### 10.19. Create pull requests with tests

* **ID**: GH-019
* **Description**: As a contributor, I want to create pull requests so that I can contribute improvements to the template.
* **Acceptance criteria**:
  * I can create a feature branch
  * I can write code and tests for my changes
  * I can push my branch and create a PR
  * GitHub Actions runs tests on my PR
  * Test results and coverage are visible in PR
  * Maintainers can review my code and tests
  * I receive feedback via PR comments

### 10.20. Handle bot errors gracefully

* **ID**: GH-020
* **Description**: As a user, I want the bot to handle errors gracefully so that I receive helpful messages instead of the bot crashing.
* **Acceptance criteria**:
  * Network errors are logged but don't crash the bot
  * Invalid commands are handled (no response or helpful message)
  * Rate limit errors are logged with appropriate warnings
  * Bot token errors cause clear error message and exit
  * Telegram API errors are logged with context
  * Bot automatically reconnects after network interruptions
  * Users never see Python stack traces
