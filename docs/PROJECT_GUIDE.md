# Django login and signup experiment
## What is actually implemented?
An early Django web application with /signup/ and /login/ forms backed by direct MySQL queries. It is not a production authentication service.

**Stack:** Python, Django 4.1.5, mysql-connector-python, HTML templates, SQLite development file and MySQL access in views.

## Source and component inventory
| File or directory | Purpose |
|---|---|
| `website/manage.py` | Django management entry point |
| `website/website/urls.py` | Routes admin/, signup/, login/ |
| `website/signup/views.py` | Form submission and direct MySQL insertion |
| `website/login/views.py` | Form login and direct MySQL lookup |
| `website/requirements.txt` | Historical dependencies |

## System diagram
~~~mermaid
flowchart LR
 User[Developer / user] --> Entry[Project UI / entry point]
 Entry --> Core[Django views]
 Core --> Data[MySQL]
~~~

## Typical interaction
~~~mermaid
sequenceDiagram
 participant User
 participant App
 participant Dependency
 User->>App: Open project / run action
 App->>Dependency: Read input or invoke dependency
 Dependency-->>App: Data / result
 App-->>User: Display output
~~~

## Installation and configuration
1. Install Python compatible with pinned Django 4.1.5; use an isolated virtual environment and review outdated dependencies before installing.
2. From website/, install requirements with python -m pip install -r requirements.txt, then run python manage.py check.
3. A working login/signup requires a reachable MySQL server and matching users table; the source currently embeds private network connection data. Do not reuse embedded credentials. Set up secure settings separately before any deployment.
4. Run python manage.py runserver for local preview only after replacing insecure database access.

~~~mermaid
flowchart TD
 Clone[Clone GitHub repository] --> Platform[Install runtime and tooling]
 Platform --> Config[Review configuration and dependencies]
 Config --> Run[Run safe local smoke test]
 Run --> Check{Expected behavior?}
 Check -->|Yes| Review[Review workflows and limitations]
 Check -->|No| Fix[Check prerequisites and known issues]
 Fix --> Config
~~~

## Usage
GET /signup/ and /login/ render templates; POST delegates to signup.views.signaction and login.views.loginaction, which operate on a MySQL users table.

## Checks to perform
- On the pull request, **GitHub Actions** validates Markdown diagram fences and guide links.
- CI performs the specified dependency-free source syntax check; this is not an application integration test.
- Manually run the steps above only in a disposable environment with permission to access any external systems.
- Record actual outputs and failures separately; this documentation does not claim unexecuted features are working.

## Limitations, risks and next improvements
- Security blocker: database credentials are embedded in view source; rotate/remove before use.
- SQL is constructed by formatting user input and passwords are handled in plaintext; SQL injection and account compromise risks.
- Global request state is unsafe under concurrent traffic; a committed SQLite database file is also present.
- No safe isolated MySQL integration test fixture is supplied, so CI only verifies Python compilation and documentation.
- No fabricated maps or performance graphs: the repository contains neither verified geographical functionality nor benchmark measurements.
- Source code changes and security fixes are outside this documentation PR.
