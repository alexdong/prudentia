# Prudentia - Your Personal Prompt, Context, and **Prompt Inspiration** Companion

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**Prudentia** is your private, local website designed to master your interactions with Large Language Models (LLMs). It's more than just a prompt manager; it's a comprehensive system to organize your prompt templates, **build a repository of inspiring prompts**, and manage context files, ensuring every conversation with AI is effective, relevant, and insightful.

## Why Prudentia? - Context, Inspiration, and Control

In the dynamic world of LLMs, simply having access isn't enough. Success lies in crafting powerful prompts and providing the right context. **Prudentia is built on the understanding that effective LLM interaction requires three key pillars: Context, Control, and Inspiration.**

*   **Context-Aware Prompting:**  Generic prompts yield generic results. **Prudentia empowers you to attach relevant context files to your prompts**, providing the LLM with the specific information it needs for informed and targeted responses.  Transform project documentation, research papers, personal notes, and code snippets into powerful context for your AI.
*   **Organized Prompt Templates:**  **Stop reinventing the wheel.** Prudentia lets you create, version, and archive your prompt templates, so you can easily reuse, refine, and share your most effective prompting strategies.  Dynamic templating using `{{ FIELD }}` makes creating adaptable prompts effortless.
*   **Centralized Context Files:**  Build a curated library of context files, always at your fingertips.  **Version and archive these files to maintain a clean, up-to-date, and highly relevant knowledge base for your AI interactions.**
*   **Focused Chat Sessions:**  Initiate chat sessions based on your best prompt templates and relevant context, keeping your conversations organized, contextualized, and easily reviewable.

**But Prudentia goes further, recognizing that great prompts often come from unexpected places.  It helps you build not just a *collection* of prompts, but a *repository of inspiration*:**

*   **Curate Inspiring Prompts:**  **Prudentia is designed to be your personal repository for collecting and organizing inspiring prompts you discover from podcasts, blog articles, research papers, or anywhere else.**  No more losing track of that perfect prompt you heard on a podcast!  Store them, categorize them, adapt them, and make them your own.
*   **Learn from the Best:**  Build your prompting skills by actively collecting and analyzing effective prompts. Prudentia becomes your learning tool, helping you understand what makes a prompt truly powerful.
*   **Jumpstart Creativity:**  When facing a blank page, browse your prompt repository for inspiration.  Adapt existing prompts to new situations, remix ideas, and overcome creative blocks with your curated collection.

**Prudentia is your personal toolkit to:**

*   **Enhance LLM Accuracy & Relevance:** Provide LLMs with context and expertly crafted prompts for superior results.
*   **Boost Productivity & Efficiency:** Streamline your workflow by quickly accessing and applying your best prompts, context, and now, your repository of prompting inspiration.
*   **Personalize & Control AI Interactions:** Tailor LLM responses to your specific needs, domain knowledge, and creative goals, all within a private and controlled environment.
*   **Build a Private Knowledge & Inspiration Base:** Curate a personal library of context files and a repository of inspiring prompts, directly accessible and usable within your LLM interactions.
*   **Easy Backup & Portability:**  **Leveraging SQLite, Prudentia keeps everything - prompts, context, chats, and your prompt repository - in a single, portable `database.db` file.**  Effortless backup and transferability for your entire AI interaction setup.

## Key Features

*   **Prompt Template Management & Inspiration Repository:**
    *   Create, edit, and delete your own prompt templates.
    *   **Import and store inspiring prompts discovered from external sources.**
    *   Version control and archiving for both your creations and your curated prompt collection.
    *   Categorization and tagging to organize your growing prompt repository.
    *   Dynamic prompt creation using `{{ FIELD }}` templating.
    *   Seamless integration with context files.
*   **Context File Library:**
    *   Manage a private library of context files, stored locally.
    *   Version control and archiving.
    *   Easy linking to prompt templates.
    *   Drag-and-drop UI for linking (**Future Enhancement**).
    *   LLM-based context file naming (**Future Enhancement**).
*   **Chat Sessions:**
    *   Create sessions based on prompt templates and context.
    *   Maintain a clear chat history.
    *   Edit session titles.
    *   Persistent context within chat sessions.
*   **Local & Private:**  Run locally for data privacy and control.
*   **Built with FastHTML & Pico CSS:** Fast and functional UI.

## Tech Stack

*   **Backend:** Python 3.12
*   **Frontend:** FastHTML, Pico CSS, Jinja2, Surreal.js
*   **Database:** SQLite
*   **LLM (Initial):** Google Gemini API

## Getting Started (Development)

1.  **Clone:** `git clone [repository URL]`
2.  **Dependencies:** (Instructions to be added)
3.  **Run:** `make run`
4.  **Browse:** `http://127.0.0.1:5000`

## Roadmap & Tool Uses - Your AI Assistant Toolkit

See [TODO.md](TODO.md) for development tasks. Prudentia aims to be your central AI hub, evolving towards:

*   Enhanced Context & **Prompt Repository Management**.
*   Broader LLM Integration.
*   Workflow Automation via LLMs.
*   Personal Knowledge Management integration.
*   Customizable Tooling & Scripting.

## License

[MIT License](LICENSE)

---

**Personal project, actively developing. Expect changes!**

Explore the code and contribute as Prudentia evolves!
