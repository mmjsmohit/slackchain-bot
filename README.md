# Slack QA (Question Answering) Bot

This bot allows users to ask queries and utilizes langchain to construct a vector database from website documentation or a list of GitHub projects.

## How It Works

To initiate a new thread, mention the bot: `@<bot name> What's ...?`

Note: It _does not_ respond to mentions intentionally to avoid sending large context back to the LLM, which would slow down performance when running entirely on CPU.

## Setting Up and Running the App Locally

To run this application on your local machine, follow these simple steps:

1. Create a new Slack app using the `manifest-dev.yml` file.
2. Install the app into your Slack workspace.
3. Retrieve your OpenAI API key at [OpenAI API Keys](https://platform.openai.com/account/api-keys).
4. Start the app.

```bash
# Create an app-level token with connections:write scope
export SLACK_APP_TOKEN=xapp-1-...
# Install the app into your workspace to grab this token
export SLACK_BOT_TOKEN=xoxb-...
# Visit https://platform.openai.com/account/api-keys for this token
export OPENAI_API_KEY=sk-...

# Optional: gpt-3.5-turbo and gpt-4 are currently supported (default: gpt-3.5-turbo)
export OPENAI_MODEL=gpt-4
# Optional: You can adjust the timeout seconds for OpenAI calls (default: 30)
export OPENAI_TIMEOUT_SECONDS=60

export MEMORY_DIR=/tmp/memory_dir

export OPENAI_API_BASE=http://localhost:8080/v1

export EMBEDDINGS_MODEL_NAME=all-MiniLM-L6-v2

## Repository and sitemap to index in the vector database on start
export SITEMAP="https://kairos.io/sitemap.xml"
export REPOSITORIES="foo,bar"
export foo_CLONE_URL="http://github.com.."
export bar_CLONE_URL="..."
export foo_BRANCH="master"
export GITHUB_PERSONAL_ACCESS_TOKEN=""
export ISSUE_REPOSITORIES="go-skynet/LocalAI,foo/bar,..."

# Optional: When the string is "true", this app translates ChatGPT prompts into a user's preferred language (default: true)
export USE_SLACK_LANGUAGE=true
# Optional: Adjust the app's logging level (default: DEBUG)
export SLACK_APP_LOG_LEVEL=INFO
# Optional: When the string is "true", translate between OpenAI markdown and Slack mrkdwn format (default: false)
export TRANSLATE_MARKDOWN=true

python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python main.py
```

## Running the App for Company Workspaces

Maintaining confidentiality of information is crucial for businesses.

As this app is open-source, feel free to fork it and deploy the app onto your own infrastructure.
After completing the local development process outlined above, deploy the app using the Dockerfile located in the root directory of this project.

The Dockerfile facilitates establishing a WebSocket connection with Slack via Socket Mode, eliminating the need for providing a public URL for communication with Slack.

## Contributions

Contributions are always welcome! 🙌

When making changes to the code in this project, please adhere to the following guidelines:

Avoid changes that could cause breaking behavior. If such changes are necessary due to critical reasons, such as security issues, initiate a discussion in GitHub Issues before making significant alterations.
Whenever possible, include unit tests, especially when modifying internals.py or adding/editing code that doesn't call any web APIs. Writing tests should be relatively straightforward.
Before committing changes, run ./validate.sh, which executes black (code formatter), flake8, and pytype (static code analyzers).
License
This project is licensed under the MIT License.
