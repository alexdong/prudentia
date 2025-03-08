# TODO.md - Prudentia - Prompt Website - FastHTML + SQLite + Pico CSS

**Project Goal:** Create a personal website (`prudentia`) to comprehensively manage prompt templates, context files with versioning and archiving, and facilitate chat with LLMs (initially Gemini), styled with Pico CSS.

**Database:** SQLite (`database.db`) - Using UUIDs for IDs.

**Tech Stack:** FastHTML, SQLite, Python 3.12, Pico CSS, Jinja2, ruff, pyright

**Coding Conventions:** Follow `CodingConvention` from `llms-ctx.txt`. Focus on readability, simplicity, and tracability. Exhaustive testing is critical.

**Project File Structure:*

```
prudentia/
├── app.py
├── database.db
├── templates/
├── static/
├── tests/
├── TODO.md
├── Makefile
├── LICENSE
├── Python.md
├── README.md
├── pyproject.toml
└── uv.lock
└── chats_module/
│ ├── urls.py
│ ├── models.py
│ ├── tests.py
│ └── templates/
└── templates_module/
│ ├── urls.py
│ ├── models.py
│ ├── tests.py
│ └── templates/
└── context_files_module/
│ ├── urls.py
│ ├── models.py
│ ├── tests.py
│ └── templates/
└── llms/
│ └── gemini.py
```


**Makefile Targets:**
*   `make lint` - Run linters (ruff).
*   `make test` - Run tests (pytest with coverage).
*   `make run` - Run the development server.
*   `make deploy` - Deploy to Railway (`fh_railway_deploy`).

**Prioritized Tasks:**

1.  **Makefile and Project Modularization - Estimated Time: 1.5 hours**
    *   1.1. Create `Makefile` with `lint`, `test`, `run`, `deploy` targets.
    *   1.2. Create `chats_module`, `templates_module`, `context_files_module` directories with `urls.py`, `models.py`, `tests.py`, `templates/` structure.
    *   1.3. Move relevant code from `app.py` into respective module files (models, routes, templates).
    *   1.4. Update `app.py` to act as main entry point, import module routes.
    *   1.5. Ensure application runs correctly after modularization.

2.  **Database Schema Design & Initialization (UUIDs, Versioning, Archiving) - Estimated Time: 1 hour**
    *   2.1. Define database schema in `models.py` for each module using UUIDs and version/archive columns (as per SPEC.md 4.1).
        *   2.1.1. `templates_module/models.py`: `PromptTemplate` model.
        *   2.1.2. `context_files_module/models.py`: `ContextFile` model.
        *   2.1.3. `chats_module/models.py`: `ChatSession`, `ChatMessage`, `TemplateContextFile` models.
    *   2.2. Update `init_db()` in `app.py` to create tables from module models.
    *   2.3. Implement `get_db_connection` and UUID helpers in shared utility or `app.py`.

3.  **Prompt Template Management (CRUD & Versioning & Archiving) - Estimated Time: 2 hours**
    *   3.1. Implement Prompt Template CRUD Functions in `templates_module/models.py`:
        *   3.1.1. `get_template(template_id, version_number=-1)` (consolidated version retrieval).
        *   3.1.2. `get_templates_list(include_archived=False)`
        *   3.1.3. `create_template(name, category, template_text)`
        *   3.1.4. `update_template(template_id)` (versioning, archiving logic).
        *   3.1.5. `delete_template(template_id)`
        *   3.1.6. `delete_template_version(template_id, version_number)`
        *   3.1.7. `archive_template_toggle(template_id)`
    *   3.2. Implement RESTful Prompt Template Routes in `templates_module/urls.py`:
        *   3.2.1. `GET /templates/` - `get_templates_list()`
        *   3.2.2. `POST /templates/` - `post_templates()`
        *   3.2.3. `GET /templates/{template_id}` - `get_template(template_id)`
        *   3.2.4. `GET /templates/{template_id}@{version_number}` - `get_template(template_id, version_number)`
        *   3.2.5. `PUT /templates/{template_id}` - `put_template(template_id)`
        *   3.2.6. `DELETE /templates/{template_id}` - `delete_template(template_id)`
        *   3.2.7. `DELETE /templates/{template_id}@{version_number}` - `delete_template_version(template_id, version_number)`
        *   3.2.8. `POST /templates/{template_id}/archive/toggle` - `post_template_archive_toggle(template_id)`
    *   3.3. Create Prompt Template HTML Templates in `templates_module/templates/`:
        *   3.3.1. `templates_module/templates/templates_list.html`
        *   3.3.2. `templates_module/templates/templates_create.html`
        *   3.3.3. `templates_module/templates/templates_view.html`
        *   3.3.4. `templates_module/templates/templates_edit.html`
        *   3.3.5. `templates_module/templates/templates_version.html`

4.  **Context File Management (CRUD & Versioning & Archiving & Template Linking) - Estimated Time: 2 hours**
    *   4.1. Implement Context File CRUD Functions in `context_files_module/models.py`:
        *   4.1.1. `get_context_file(context_file_id, version_number=-1)`
        *   4.1.2. `get_context_files_list(include_archived=False)`
        *   4.1.3. `create_context_file(file_content)`
        *   4.1.4. `update_context_file(context_file_id)`
        *   4.1.5. `delete_context_file(context_file_id)`
        *   4.1.6. `delete_context_file_version(context_file_id, version_number)`
        *   4.1.7. `archive_context_file_toggle(context_file_id)`
    *   4.2. Implement RESTful Context File Routes in `context_files_module/urls.py`:
        *   4.2.1. `GET /context_files/` - `get_context_files_list()`
        *   4.2.2. `POST /context_files/` - `post_context_files()`
        *   4.2.3. `GET /context_files/{context_file_id}` - `get_context_file(context_file_id)`
        *   4.2.4. `GET /context_files/{context_file_id}@{version_number}` - `get_context_file(context_file_id, version_number)`
        *   4.2.5. `PUT /context_files/{context_file_id}` - `put_context_file(context_file_id)`
        *   4.2.6. `DELETE /context_files/{context_file_id}` - `delete_context_file(context_file_id)`
        *   4.2.7. `DELETE /context_files/{context_file_id}@{version_number}` - `delete_context_file_version(context_file_id, version_number)`
        *   4.2.8. `POST /context_files/{context_file_id}/archive/toggle` - `post_context_file_archive_toggle(context_file_id)`
        *   4.2.9. `POST /templates/{template_id}/context_files` - `post_template_context_files(template_id)`
        *   4.2.10. `DELETE /templates/{template_id}/context_files/{context_file_id}` - `delete_template_context_file(template_id)`
    *   4.3. Create Context File HTML Templates in `context_files_module/templates/`:
        *   4.3.1. `context_files_module/templates/context_files_list.html`
        *   4.3.2. `context_files_module/templates/context_files_create.html`
        *   4.3.3. `context_files_module/templates/context_files_view.html`
        *   4.3.4. `context_files_module/templates/context_files_edit.html`
        *   4.3.5. `context_files_module/templates/context_files_version.html`

5.  **Chat Session Creation & Basic Display - Estimated Time: 1.5 hours**
    *   5.1. Database Functions in `chats_module/models.py`:
        *   5.1.1. `create_chat_session(prompt_template_id, title="Untitled")`
        *   5.1.2. `get_chat_sessions_list()`
        *   5.1.3. `get_chat_session(chat_session_id)`
    *   5.2. Chat Session Routes in `chats_module/urls.py`:
        *   5.2.1. `GET /chats/create/` - `get_chats_create_prompt_selector()`
        *   5.2.2. `GET /chats/create/{template_id}/` - `get_chats_create_input_form(template_id)`
        *   5.2.3. `GET /` (Homepage) - `get_index()`
        *   5.2.4. `GET /chats/{chat_session_id}` - `get_chat_session(chat_session_id)`
        *   5.2.5. `PUT /chats/{chat_session_id}/title` - `put_chat_session_title(chat_session_id)`
    *   5.3. Create Chat Session HTML Templates in `chats_module/templates/`:
        *   5.3.1. `chats_module/templates/index.html` (Homepage).
        *   5.3.2. `chats_module/templates/chats_create_prompt_selector.html`
        *   5.3.3. `chats_module/templates/chats_create_input_form.html`
        *   5.3.4. `chats_module/templates/chats_view.html`
        *   5.3.5. `chats_module/templates/chats_edit_title.html`

6.  **Basic Chat Interaction (Gemini API Placeholder & Message Display) - Estimated Time: 1 hour**
    *   6.1. Jinja2 templating for prompt rendering in `chats_module/models.py` or `llms/gemini.py`.
    *   6.2. Gemini API call placeholder in `llms/gemini.py`. Define uniform interface in `llms/__init__.py`.
    *   6.3. Database Functions in `chats_module/models.py`:
        *   6.3.1. `add_chat_message(chat_session_id, sequence_number, role, user_input, llm_response, llm_provider)`
        *   6.3.2. `get_chat_messages_list(chat_session_id)`
    *   6.4. Chat Interaction Route in `chats_module/urls.py`:
        *   6.4.1. `POST /chats/{chat_session_id}/messages` - `post_chat_session_messages(chat_session_id)`
    *   6.5. Update `chats_module/templates/chats_view.html`:
        *   6.5.1. Form for message input to `POST /chats/{chat_session_id}/messages`.
        *   6.5.2. Display user/assistant messages in text log.
        *   6.5.3. Cmd+Enter submission for textarea (JS).

7.  **Pico CSS Integration & Basic Styling - Estimated Time: 1 hour**
    *   7.1. Integrate Pico CSS.
    *   7.2. Link Pico CSS in `templates/layout.html`.
    *   7.3. Apply basic Pico CSS styling to templates using FastHTML Pico components.

8.  **Context File Linking to Templates UI (Basic Select/Deselect) - Estimated Time: 1 hour**
    *   8.1. Update Prompt Template Create/Edit Templates (`templates_module/templates/templates_create.html`, `templates_module/templates/templates_edit.html`):
        *   8.1.1. Display list of context files (checkboxes/select).
        *   8.1.2. Allow selection/deselection, update backend logic (`template_context_files`).
    *   8.2. Update Prompt Template View Template (`templates_module/templates/templates_view.html`):
        *   8.2.1. Display associated context files.

9.  **Basic Test Suite Setup - Estimated Time: 30 minutes**
    *   9.1. Create basic tests in `tests/` and module `tests.py` files using `pytest`.
    *   9.2. Test core CRUD functions, Jinja2 rendering, basic database interactions, RESTful URLs.

**Stretch Goals (Critical Experience Enhancements):**

10. **Automatic Chat Title Generation from LLM:**
    *   10.1. Implement LLM-based chat title generation in `chats_module/models.py` or `llms/gemini.py`.
    *   10.2. Integrate title generation into `create_chat_session` and `post_chat_session_messages` flows.
    *   10.3. Update UI to display and edit automatically generated titles.

11. **Implement Drag-and-Drop UI for Context File Linking to Templates (Multiple Files, LLM Naming):**
    *   11.1. Drag-and-drop zone in `templates_module/templates/templates_create.html`, `templates_module/templates/templates_edit.html`.
    *   11.2. Support multiple file uploads.
    *   11.3. Backend route to handle file uploads and linking.
    *   11.4. LLM-based context file naming suggestion (optional edit before save).
    *   11.5. Update UI to display drag-and-dropped and linked files.

12. **Left Sidebar Navigation with Accordion (FastHTML & Pico CSS):**
    *   12.1. Implement sidebar structure in `templates/layout.html` (Pico CSS, FastHTML components).
    *   12.2. Accordion menu for Templates, Context Files, Chats, etc. (FastHTML Group/Card).
    *   12.3. HTMX for dynamic accordion behavior (if needed).
    *   12.4. Responsive sidebar (Pico CSS Grid/Container).
    *   12.5. Update all templates to use sidebar navigation.

**Note:** This TODO list is detailed and numbered for easy reference. Prioritize tasks sequentially. Test iteratively. Pico CSS styling and Stretch Goals after core functionality.
