# Veyra OS

## Product Requirements Document — V2

**Product:** Veyra OS
**Repository:** `veyra-os`
**Type:** Virtual Operating System
**Platforms:** Web + Electron Desktop
**Frontend:** Next.js
**Backend:** Next.js
**Database:** PostgreSQL / Neon
**Authentication:** JWT
**Desktop Runtime:** Electron

---

# 1. Product Vision

**Veyra OS is a virtual operating system that runs inside a browser or desktop application.**

The experience should not feel like a normal web application.

The user should feel as though they have:

> **Booted a computer.**

There are two distinct experiences:

```text
                    VEYRA
                      │
             ┌────────┴────────┐
             │                 │
       Veyra Website       Veyra OS
       Landing Page       Virtual Computer
             │                 │
       Marketing / Info     Boot Experience
                             │
                         Login / Desktop
```

The marketing website and operating system are **separate applications and separate URLs**.

---

# 2. Product Structure

Veyra consists of two primary applications.

## 2.1 Veyra Website

The public-facing marketing website.

Example:

```text
https://veyra-os.com
```

Purpose:

* Introduce Veyra OS
* Explain features
* Show screenshots/video
* Explain technology
* Provide documentation
* Provide GitHub link
* Allow users to enter Veyra OS

Primary CTA:

> **Enter Veyra OS**

Clicking the CTA takes the user to the actual OS.

Example:

```text
veyra-os.com
       │
       │ Enter Veyra OS
       ▼
os.veyra-os.com
```

---

# 3. Veyra OS Application

The actual virtual operating system lives separately.

Example:

```text
https://os.veyra-os.com
```

This application should have **no conventional website navigation**.

When the user enters the application, they should experience a virtual boot sequence.

---

# 4. Boot Experience

The boot process is one of the defining features of Veyra OS.

The user should not immediately see the desktop.

Instead:

```text
OS URL
   │
   ▼
Boot Screen
   │
   ▼
System Initialization
   │
   ▼
Login / User Selection
   │
   ▼
Desktop
```

The experience should visually resemble a computer starting up.

However, this is entirely virtual.

There is no real BIOS, firmware, kernel, or hardware boot process.

---

# 5. Virtual Boot Sequence

Example sequence:

```text
┌──────────────────────────────────────────────┐
│                                              │
│                    VEYRA                     │
│                                              │
│                    ◉                         │
│                                              │
│             Starting Veyra OS...             │
│                                              │
│                  ━━━━━━━━                    │
│                                              │
└──────────────────────────────────────────────┘
```

Then:

```text
Initializing system...
Loading kernel...
Loading services...
Loading user environment...
Starting desktop...
```

These are **simulated operating-system states**.

The system should not claim that an actual operating-system kernel is being loaded.

---

# 6. Boot State Machine

The application should implement an explicit boot state machine.

```text
BOOT
 │
 ▼
INITIALIZING
 │
 ▼
LOADING_SYSTEM
 │
 ▼
LOADING_USER_ENVIRONMENT
 │
 ▼
AUTHENTICATION
 │
 ▼
DESKTOP
```

Possible failure state:

```text
ANY STATE
   │
   ▼
ERROR
   │
   ▼
RECOVERY / RETRY
```

This should be represented in application state rather than being a collection of arbitrary timers.

---

# 7. Returning Users

If a valid authenticated session exists:

```text
OS URL
   ↓
Boot
   ↓
Session detected
   ↓
User environment loaded
   ↓
Desktop
```

If no valid session exists:

```text
OS URL
   ↓
Boot
   ↓
Login
```

The user should not have to repeatedly navigate through the marketing website.

---

# 8. Login Experience

The login screen should still feel like part of an operating system.

Instead of a conventional SaaS login page:

```text
Email
Password
[ Login ]
```

Veyra should present a virtual login screen.

Example:

```text
┌──────────────────────────────────────────────┐
│                                              │
│                    VEYRA                     │
│                                              │
│                  👤 Sheikh                   │
│                                              │
│                 •••••••••                   │
│                                              │
│                   [ Enter ]                  │
│                                              │
└──────────────────────────────────────────────┘
```

For users without an account:

> Create Veyra account

This can open the registration flow.

---

# 9. Desktop Environment

After successful authentication:

```text
Login
  ↓
Loading User Environment
  ↓
Desktop
```

The desktop should be the primary application environment.

It should visually and behaviorally resemble a modern desktop operating system.

---

# 10. Desktop Requirements

The desktop must support:

* Wallpaper
* Desktop icons
* Application windows
* Taskbar
* Application launcher
* System tray
* Clock
* Notifications
* Context menu
* Window focus
* Multiple simultaneous applications

Example:

```text
┌───────────────────────────────────────────────────────────┐
│ Veyra                                      10:42   WiFi 🔋 │
├───────────────────────────────────────────────────────────┤
│                                                           │
│   📁 Files          📝 Notes         🌐 Browser            │
│                                                           │
│   💻 Terminal       ⚙ Settings                           │
│                                                           │
│                                                           │
│                                                           │
│                                                           │
├───────────────────────────────────────────────────────────┤
│ ◉  📁  📝  🌐  💻  ⚙                         🔔  10:42   │
└───────────────────────────────────────────────────────────┘
```

---

# 11. Window System

Applications must run inside virtual windows.

Example:

```text
┌─────────────────────────────────────────┐
│ Files                         ─ □ ×     │
├─────────────────────────────────────────┤
│                                         │
│ Documents                               │
│ Projects                                │
│ Downloads                               │
│                                         │
└─────────────────────────────────────────┘
```

Windows must support:

* Drag
* Resize
* Minimize
* Maximize
* Restore
* Close
* Focus
* Z-index
* Snap behavior

Multiple applications can exist simultaneously.

Example:

```text
Desktop
│
├── Files Window
├── Browser Window
├── Terminal Window
└── Notes Window
```

---

# 12. Application Model

Every Veyra application should behave like an OS application.

Initial applications:

```text
Veyra Apps
│
├── Files
├── Notes
├── Terminal
├── Browser
├── Settings
└── Task Manager
```

Each application should have:

* Application ID
* Name
* Icon
* Window configuration
* Entry component
* Permissions where applicable

Example conceptual structure:

```text
Application
│
├── id
├── name
├── icon
├── component
├── defaultWidth
├── defaultHeight
└── permissions
```

This makes it possible to add applications later without rewriting the desktop.

---

# 13. Start / Application Menu

The application launcher should behave like an operating-system application menu.

Example:

```text
┌─────────────────────────────────────────┐
│ Search applications...                  │
│                                         │
│ 📁 Files       📝 Notes                 │
│ 💻 Terminal    🌐 Browser               │
│ ⚙ Settings    📊 Task Manager           │
│                                         │
│                         👤 Sheikh       │
│                         ⏻ Power         │
└─────────────────────────────────────────┘
```

---

# 14. Virtual Power Menu

Since Veyra is an OS simulation, it should have virtual power controls.

Options:

```text
Power
│
├── Lock
├── Log Out
├── Restart Veyra
└── Shut Down
```

These actions do not shut down the user's actual computer.

They manipulate the Veyra application state.

### Restart

```text
Desktop
 ↓
Restart
 ↓
Boot Screen
 ↓
Initialization
 ↓
Login / Session
 ↓
Desktop
```

### Shut Down

```text
Desktop
 ↓
Shut Down
 ↓
Veyra shutdown screen
 ↓
"Veyra OS is powered off"
```

The user can then click:

> **Power On**

which starts the virtual boot sequence again.

This is an important part of the experience.

---

# 15. Lock Screen

Locking Veyra should not log the user out.

Example:

```text
┌──────────────────────────────────────────────┐
│                                              │
│                   10:42 AM                   │
│                September 27                  │
│                                              │
│                    👤                        │
│                   Sheikh                     │
│                                              │
│                  ••••••••                    │
│                                              │
│                 Unlock →                     │
│                                              │
└──────────────────────────────────────────────┘
```

---

# 16. Files Application

The Files application represents Veyra's virtual filesystem.

Users can:

* Create folders
* Create files
* Rename items
* Delete items
* Move items
* Search
* Sort
* Open files
* View metadata

The filesystem is virtual and backed by Veyra's backend/database/storage architecture.

---

# 17. Notes Application

Notes behaves as a native Veyra application.

Features:

* Create
* Edit
* Delete
* Rename
* Search
* Autosave
* Last modified timestamp

Notes belong to the authenticated user.

---

# 18. Terminal

Veyra Terminal is a **virtual terminal**.

It must never expose unrestricted server or host-machine command execution.

Example:

```text
veyra@sheikh:~$ ls

Documents
Projects
Downloads
Notes

veyra@sheikh:~$ cd Projects

veyra@sheikh:~/Projects$
```

Commands operate against Veyra's virtual environment.

Initial commands:

```text
help
clear
pwd
ls
cd
mkdir
touch
cat
echo
```

---

# 19. Browser

Veyra Browser provides an application-like browsing environment.

Initial capabilities:

* Address bar
* Search
* Back
* Forward
* Reload
* History
* External websites

The Electron version can eventually provide additional browser capabilities.

---

# 20. Settings

Settings should feel like an actual OS settings application.

Categories:

```text
Settings
│
├── Personalization
├── Display
├── Desktop
├── Applications
├── Account
├── Security
├── Notifications
└── System
```

---

# 21. Backend Architecture

The OS frontend and website will communicate with the Next.js backend.

```text
Veyra Website
       │
       │
Veyra OS Web
       │
       │
Veyra Electron
       │
       ▼
 Next.js Backend
       │
       ├── Authentication
       ├── Users
       ├── Files
       ├── Notes
       ├── Settings
       ├── Desktop
       └── Applications
       │
       ▼
 PostgreSQL / Neon
```

---

# 22. URL Architecture

The public website and OS should be clearly separated.

Recommended:

```text
veyra-os.com
```

Marketing website.

```text
os.veyra-os.com
```

Virtual operating system.

Potential future services:

```text
docs.veyra-os.com
api.veyra-os.com
status.veyra-os.com
```

The exact domain can change, but the architectural separation should remain.

---

# 23. Website

The website is **not part of the OS application**.

It should contain:

## Hero

> **Meet Veyra OS**

> A virtual operating environment built for the web and desktop.

CTA:

> **Enter Veyra OS**

Secondary CTA:

> **View on GitHub**

---

## Product Showcase

Show:

* Boot screen
* Desktop
* Windows
* Files
* Terminal
* Settings
* Electron application

---

## Architecture

Explain:

```text
Next.js
Electron
PostgreSQL
Neon
JWT
```

---

## Features

Highlight:

* Virtual Desktop
* Window Manager
* Cloud Files
* Virtual Terminal
* Applications
* Persistent Workspace
* Web + Desktop

---

# 24. Authentication Architecture

Authentication is shared between the OS and backend.

```text
                  Authentication
                        │
             ┌──────────┴──────────┐
             │                     │
        Veyra Web              Veyra OS
             │                     │
             └──────────┬──────────┘
                        │
                     JWT
                        │
                        ▼
                 Next.js Backend
                        │
                        ▼
                    PostgreSQL
```

Authentication should use secure session handling.

JWTs should not be unnecessarily exposed to client-side JavaScript or stored in `localStorage`.

---

# 25. Database

Initial tables:

```text
users
sessions
settings
desktop_items
notes
files
```

Potential future tables:

```text
applications
notifications
recent_items
user_app_settings
file_permissions
```

---

# 26. Electron Application

Electron is the **desktop distribution of Veyra OS**, not a second implementation of the OS.

```text
                Veyra OS
                   │
          ┌────────┴────────┐
          │                 │
       Web App          Electron App
          │                 │
          └────────┬────────┘
                   │
              Shared Backend
```

The same Veyra account should work across both.

The Electron application should provide:

* Native desktop window
* Veyra OS boot experience
* Native application lifecycle
* Secure IPC
* System tray
* Desktop notifications where appropriate
* Future native filesystem capabilities

---

# 27. Boot vs Actual Operating System

Veyra must clearly be a **virtual operating system**.

The project does not attempt to implement:

* BIOS
* Bootloader
* Kernel
* Device drivers
* Hardware process scheduler
* Memory management
* Actual filesystem kernel
* CPU scheduling

Instead, Veyra **simulates the user experience and operating-system abstractions at the application layer**.

This distinction should be documented in the project.

---

# 28. V1 Scope

V1 should focus on the complete operating-system experience.

### Website

* Landing page
* Product explanation
* Feature showcase
* Architecture
* GitHub link
* Enter OS CTA

### OS

* Boot screen
* Initialization sequence
* Login
* Lock screen
* Desktop
* Taskbar
* Application launcher
* Windows
* Window manager
* Virtual power menu
* Files
* Notes
* Terminal
* Browser
* Settings
* Task Manager

### Backend

* Registration
* Login
* JWT/session management
* User data
* Notes
* Files
* Settings
* Desktop state

### Desktop

* Electron shell
* Boot experience
* Shared Veyra application

---

# 29. Veyra OS Lifecycle

The complete lifecycle should look like:

```text
                 ┌─────────────┐
                 │ Veyra Website│
                 └──────┬──────┘
                        │
                  Enter Veyra OS
                        │
                        ▼
                 ┌─────────────┐
                 │   Boot      │
                 └──────┬──────┘
                        │
                        ▼
                 ┌─────────────┐
                 │ Initialize  │
                 └──────┬──────┘
                        │
                        ▼
                 ┌─────────────┐
                 │ Authenticate│
                 └──────┬──────┘
                        │
                        ▼
                 ┌─────────────┐
                 │  Desktop    │
                 └──────┬──────┘
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
       Apps          Settings       Files
          │
          ▼
       Windows
          │
          ▼
       Lock / Logout / Restart / Shutdown
```

---

# 30. Core UX Principle

The most important product principle is:

> **When the user enters Veyra OS, they should feel like they are entering a computer, not navigating a website.**

Therefore:

* No normal website navbar inside the OS.
* No traditional dashboard layout.
* Applications open as windows.
* The desktop persists.
* The taskbar remains available.
* Power actions affect the virtual OS.
* Restart returns to the boot sequence.
* Lock returns to the lock screen.
* Shutdown presents a powered-off state.
* The OS has its own visual identity.

---

# 31. Recommended Application Separation

For maintainability, use three conceptual projects:

```text
veyra/
│
├── website/
│   └── Marketing Website
│
├── os/
│   └── Veyra OS Next.js Application
│
├── desktop/
│   └── Electron Shell
│
└── backend/
    └── Shared API / services
```

Depending on implementation, the website and OS can still live in a monorepo.

Recommended long-term structure:

```text
veyra/
│
├── apps/
│   ├── website/
│   ├── os/
│   └── desktop/
│
├── packages/
│   ├── ui/
│   ├── auth/
│   ├── database/
│   ├── api/
│   └── types/
│
└── docs/
```

This gives Veyra a proper product architecture rather than one enormous Next.js application.

---

# 32. V1 Success Criteria

A user should be able to perform this complete journey:

```text
Open veyra-os.com
       ↓
Understand Veyra OS
       ↓
Click "Enter Veyra OS"
       ↓
os.veyra-os.com
       ↓
See Veyra boot
       ↓
Login
       ↓
Enter desktop
       ↓
Open Files
       ↓
Open Notes
       ↓
Open Terminal
       ↓
Open Browser
       ↓
Move/resize multiple windows
       ↓
Change settings
       ↓
Lock OS
       ↓
Unlock OS
       ↓
Restart Veyra
       ↓
See boot screen again
       ↓
Return to desktop
       ↓
Shut down Veyra
       ↓
See powered-off screen
       ↓
Power Veyra on again
```

If this entire flow works smoothly, **Veyra OS V1 is a complete product experience**.

---

# 33. Product Definition

### One-line definition

> **Veyra OS is a virtual operating system that brings a persistent desktop computing experience to the web and desktop.**

### Product structure

```text
VEYRA
│
├── Website
│   └── Marketing + Product
│
└── Veyra OS
    │
    ├── Boot
    ├── Login
    ├── Desktop
    ├── Windows
    ├── Applications
    ├── Files
    ├── Terminal
    ├── Settings
    ├── Lock
    ├── Restart
    └── Shutdown
```

### Core philosophy

> **Veyra doesn't imitate a website that looks like an OS. Veyra creates a virtual computing environment inside the web.**
