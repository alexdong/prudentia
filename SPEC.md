## 1. Introduction

This document specifies the requirements, architecture, and implementation details for "Prudentia," a personal website designed to manage prompt templates, context files, and facilitate interactions with Large Language Models (LLMs).

## 2. Requirements

### 2.1. Core Functionality

#### 2.1.1. Prompt Template Management

*   2.1.1.1. Create, Read, Update, and Delete (CRUD) prompt templates.
*   2.1.1.2. Versioning of prompt templates: track changes and revert to previous versions.
*   2.1.1.3. Archiving of prompt templates: ability to toggle archive status (archive/unarchive).
*   2.1.1.4. Deletion of specific versions of prompt templates.
*   2.1.1.5. Templated fields within prompt templates using notation: `{{ FIELD }}` or `{{ FIELD:type }}`.
    *   2.1.1.5.1. If `:type` is omitted, default input type is text.
    *   2.1.1.5.2. Supported `:type` values:
        *   `:` or `:text`: Standard text input field (`<input type="text">`).
        *   `:textarea`: Textarea input field (`<textarea>`).
        *   `:input`: Generic input field (defaults to text, can be extended for other input types in future).
        *   `:image`: Image upload/preview field (future enhancement - for now treat as text).
        *   `:upload`: Generic file upload field (future enhancement - for now treat as text).

#### 2.1.2. Context File Management

*   2.1.2.1. CRUD operations for context files.
*   2.1.2.2. Versioning and archiving for context files, similar to templates.
*   2.1.2.3. Deletion of specific versions of context files.
*   2.1.2.4. Storage of context file content directly in the SQLite database.
*   2.1.2.5. Linking context files to prompt templates (many-to-many relationship).
*   2.1.2.6. Drag-and-drop UI for linking multiple context files to templates (Stretch Goal).
*   2.1.2.7. Optional LLM-based naming suggestion for uploaded context files (Stretch Goal).

#### 2.1.3. Chat Session Management

*   2.1.3.1. Creation of chat sessions based on prompt templates.
*   2.1.3.2. Descriptive titles for chat sessions (initially "Untitled," with automatic LLM-generated titles as a Stretch Goal).
*   2.1.3.3. Display of chat session history as a text log.
*   2.1.3.4. Storage of chat messages (user input and LLM responses) in the database.
*   2.1.3.5. Ability to edit chat session titles.

#### 2.1.4. LLM Interaction

*   2.1.4.1. Integration with Google Gemini API (initially, with modular design for future API switching).
*   2.1.4.2. Placeholder Gemini API calls for initial development (echoing user input).
*   2.1.4.3. Jinja2 templating for rendering prompt templates before sending to LLM.
*   2.1.4.4. Display of LLM responses in Markdown format (if possible, or plain text).

#### 2.1.5. User Interface (UI)

*   2.1.5.1. Web-based UI using FastHTML and Pico CSS.
*   2.1.5.2. Left sidebar navigation with accordion menu for Templates, Context Files, and Chats (Stretch Goal).
*   2.1.5.3. Homepage displaying recent prompt templates and chat sessions.
*   2.1.5.4. Dynamic forms generated based on `{{ FIELD }}` or `{{ FIELD:type }}` delimiters in prompt templates.
*   2.1.5.5. Cmd+Enter submission for chat input textarea.
*   2.1.5.6. Clear UI for version navigation and archiving management (using `<` `>` style version selectors).

### 2.2. Non-Functional Requirements

*   2.2.1. Performance: Responsive and performant for personal use.
*   2.2.2. Scalability: Not a primary concern, designed for personal use, but should be reasonably efficient.
*   2.2.3. Security: Basic web application security practices for local use. No login required.
*   2.2.4. Maintainability: Code should be readable, well-structured, and easy to maintain (following `CodingConvention`).
*   2.2.5. Testability: Comprehensive test suite for all core functionalities.

### 2.3. Further Features (Stretch Goals)

*   2.3.1. Automatic Chat Title Generation from LLM.
*   2.3.2. Implement Drag-and-Drop UI for Context File Linking to Templates (Multiple Files, LLM Naming).
*   2.3.3. Left sidebar navigation with accordion menu for Templates, Context Files, and Chats.
*   2.3.4. Categorization of prompt templates, context files, and chats (auto-organize feature).

## 3. Architecture

### 3.1. Technology Stack

*   3.1.1. Frontend Framework: FastHTML (server-rendered hypermedia).
*   3.1.2. CSS Framework: Pico CSS.
*   3.1.3. Templating Engine: Jinja2 (for prompt template rendering).
*   3.1.4. Backend Language: Python 3.12.
*   3.1.5. Web Server: Uvicorn (via FastHTML's `serve()` utility).
*   3.1.6. Database: SQLite (`database.db`) with `fastlite` or default `sqlite3` Python library.
*   3.1.7. LLM API: Google Gemini API (initially).
*   3.1.8. Javascript Library: Surreal.js (included with FastHTML) for frontend interactivity.
*   3.1.9. Development Tools: `ruff`, `pyright`.

### 3.2. Module Structure

The project is modularized into the following directories:

*   `app.py`: Core application setup, routing, database initialization.
*   `templates/`: Global layout and homepage templates.
*   `static/`: Static files (CSS, JS, images).
*   `tests/`: Test suite directory.
*   `chats_module/`: Chat session functionality (models, URLs, templates, tests).
*   `templates_module/`: Prompt template management (models, URLs, templates, tests).
*   `context_files_module/`: Context file management (models, URLs, templates, tests).
*   `llms/`: LLM API integration (e.g., `gemini.py`).

### 3.3. Data Flow

1.  User Interaction: User interacts with the web UI.
2.  FastHTML Routing: FastHTML routes the request to the appropriate Python handler function.
3.  Backend Logic:
    *   Handler function interacts with the database.
    *   For chat interactions, handler function: retrieves prompt template/context, renders prompt, calls LLM API, stores messages.
    *   Handler function generates a FastHTML response.
4.  HTML Rendering: FastHTML renders the FT response into HTML.
5.  Client-Side Update (HTMX): HTMX updates the web page.

## 4. Data Handling

### 4.1. Database Schema (SQLite - `database.db`)

*   **`prompt_templates` Table:**
    *   `id` (UUID, PRIMARY KEY)
    *   `version` (INTEGER)
    *   `name` (TEXT, NOT NULL)
    *   `category` (TEXT)
    *   `template_text` (TEXT, NOT NULL)
    *   `archived` (BOOLEAN, DEFAULT FALSE)

*   **`context_files` Table:**
    *   `id` (UUID, PRIMARY KEY)
    *   `version` (INTEGER)
    *   `name` (TEXT, NOT NULL)
    *   `category` (TEXT)
    *   `content` (TEXT, NOT NULL)
    *   `archived` (BOOLEAN, DEFAULT FALSE)

*   **`chat_sessions` Table:**
    *   `id` (UUID, PRIMARY KEY)
    *   `title` (TEXT, NOT NULL)
    *   `created_at` (TIMESTAMP)
    *   `updated_at` (TIMESTAMP)
    *   `prompt_template_id` (UUID, FOREIGN KEY referencing `prompt_templates.id`)

*   **`chat_messages` Table:**
    *   `id` (UUID, PRIMARY KEY)
    *   `chat_session_id` (UUID, FOREIGN KEY referencing `chat_sessions.id`)
    *   `sequence_number` (INTEGER)
    *   `created_at` (TIMESTAMP)
    *   `role` (TEXT)
    *   `user_input` (TEXT)
    *   `llm_response` (TEXT)
    *   `llm_provider` (TEXT)

*   **`template_context_files` Table:**
    *   `template_id` (UUID, FOREIGN KEY referencing `prompt_templates.id`)
    *   `context_file_id` (UUID, FOREIGN KEY referencing `context_files.id`)
    *   PRIMARY KEY (`template_id`, `context_file_id`)

### 4.2. Versioning and Archiving

*   **Versioning:** New versions on update, old versions are retained.
*   **Archiving:** Toggleable archive status using `archived` flag.

### 4.3. Data Flow (CRUD Operations)

*   Module `models.py` for database interactions.
*   Module `urls.py` calls model functions for requests and rendering data.

## 5. Error Handling

*   Basic error display using FastHTML debug mode during development.
*   User-friendly error pages and logging for production (future).

## 6. Testing Plan

*   Testing Framework: `pytest` with `pytest-cov`.
*   Test Scope: Unit tests for CRUD, database models, Jinja2 rendering, routing, LLM placeholder.
*   Test Organization: `tests/` directory and module-specific `tests.py` files.
*   Test Execution: `make test` command.
*   Coverage Goal: High test coverage.

## 7. REST API Endpoints (Formalized HTTP Verb Prefix)

Function names in `urls.py` prefixed with HTTP verb. Versioned entities accessed via `/{entity_id}@{version_number}` notation.

*   **Prompt Templates:**
    *   `GET /templates/` - `get_templates_list()`
    *   `POST /templates/` - `post_templates()`
    *   `GET /templates/{template_id}` - `get_template(template_id)` (latest version)
    *   `GET /templates/{template_id}@{version_number}` - `get_template(template_id, version_number)` (specific version)
    *   `PUT /templates/{template_id}` - `put_template(template_id)`
    *   `DELETE /templates/{template_id}` - `delete_template(template_id)`
    *   `DELETE /templates/{template_id}@{version_number}` - `delete_template_version(template_id, version_number)` (delete specific version)
    *   `POST /templates/{template_id}/archive/toggle` - `post_template_archive_toggle(template_id)` (toggle archive status)
*   **Context Files:**
    *   `GET /context_files/` - `get_context_files_list()`
    *   `POST /context_files/` - `post_context_files()`
    *   `GET /context_files/{context_file_id}` - `get_context_file(context_file_id)` (latest version)
    *   `GET /context_files/{context_file_id}@{version_number}` - `get_context_file(context_file_id, version_number)` (specific version)
    *   `PUT /context_files/{context_file_id}` - `put_context_file(context_file_id)`
    *   `DELETE /context_files/{context_file_id}` - `delete_context_file(context_file_id)`
    *   `DELETE /context_files/{context_file_id}@{version_number}` - `delete_context_file_version(context_file_id, version_number)` (delete specific version)
    *   `POST /context_files/{context_file_id}/archive/toggle` - `post_context_file_archive_toggle(context_file_id)` (toggle archive status)
*   **Chats:**
    *   `GET /` - `get_index()` (Homepage - recent chats)
    *   `GET /chats/create/` - `get_chats_create_prompt_selector()`
    *   `GET /chats/create/{template_id}/` - `get_chats_create_input_form(template_id)`
    *   `GET /chats/{chat_session_id}` - `get_chat_session(chat_session_id)`
    *   `PUT /chats/{chat_session_id}/title` - `put_chat_session_title(chat_session_id)`
    *   `POST /chats/{chat_session_id}/messages` - `post_chat_session_messages(chat_session_id)`

## 8. UI Version Navigation

Version navigation using `<` `>` style version selectors within view pages, displaying the latest version by default. Backend `get_prompt_template` and `get_context_file` functions handle version retrieval based on optional `version_number` parameter.
