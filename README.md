# Prudentia - Personal LLM UI

**Prudentia** is your private, local web app for interacting with Large Language Models (LLMs). 
It

1. maintains knowledge base and a library of context files. Prudentia
   centralizes Knowledge and Context Files and keep them always in-sync with
   Dropbox, OneDrive and other cloud storage systems. 

2. organizes your prompt templates. Prudentia lets you create and version your
   prompt templates, so you can easily reuse, refine, and share your most
   effective prompts.  Further, generic prompts yield generic results.
   Prudentia provides the structure to incorporate knowledge and contexts in
   your prompts.

3. provides an appstore of 3rd party software that have been vetted and made
   available through tool use. `browse("markdown")` will open the link in a headless
   browser and return the text as markdown format.

## Key Features

 * Prompt Template Management & Inspiration Repository:
     - Create, edit, and delete your own prompt templates.
     - Categorization and tagging to organize your growing prompt repository.
     - Link in context files and knowledge base entries.
     - Prompt creation following jinja2's format like `{{ FIELD|format(int) }}`

 * Knowledge Base & Context File Library:
     - Manage a private library of context files, stored locally.
     - Manage a collection of "knowledge" about your interests, projects, and goals. 
     - Easy linking into prompt templates.
     - Drag-and-drop UI for linking
     - LLM-based context file naming

 * Chat Sessions:
     - Create sessions based on prompt templates and context.
     - Maintain a clear chat history.
     - Edit session titles.
     - Persistent context within chat sessions.
     - Review auto-extracted knowledge entries from chat sessions.

 * Local & Private: Run locally for data privacy and control. The database is a
   single SQLite file. Backup and restore is as simple as hard link the file
   into Dropbox.


## Tech Stack

 * Backend: Python 3.12
 * Frontend: FastHTML, Pico CSS, Jinja2, Surreal.js
 * Database: SQLite via fastlite
 * Dev Tools: uv, ruff, pytest, black
 * Deployment: Docker, Docker Compose
 * LLM (Initial): Google Gemini API


## Roadmap

* Personal Knowledge Base. (Auto extraction of knowledge from chat sessions.)
* Multiple-steps prompts. (Software product roadmap. Harper.blog prompts)
* Add a current time tool. Make the code compatible with [Open WebUI Tools
  Spec](https://docs.openwebui.com/features/plugin/tools/)
* Add a browser tool. Example: {{ "https://lobste.rs/"|browser("markdown",
  "instructions") }} where the instructions are LLM instructions 
  as described by github.com/browserbase/stagehand.
* Add a search tool. Example: {{ "Straussian interpreation"|search("google") }}


## License

[MIT License](LICENSE)

