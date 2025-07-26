interface_module/README.md

Module Name  
Interface Module

Purpose  
Handles all outbound communication from the model to the operating system.  
Used for OS-level actions such as file creation, saving, deletion, and execution.  
Called only by the final inference layer (form_thread) after output is confirmed and shaped.

Functions Exposed  
These are exposed via __init__.py for direct access by form_thread.interface_module.

create(file, type)  
- Creates a new file or system object.  
- Type defines handler logic (.txt, .json, .py, etc).  
- Logs file name, type, and origin module.

save(file, path)  
- Saves a file to a specified system path.  
- Overwrites if file exists.  
- Validates path, permissions, and file integrity.

delete(path)  
- Deletes a file or folder at given path.  
- Permanent unless rollback module is active.  
- Logs deletion request and result.

Permissions  
- Only accessible from form_thread.  
- Model may only invoke via structured function call format.  
- Prevents accidental or implicit system modification.

Dependencies  
- os, shutil, pathlib (Python standard libraries)  
- log_thread for persistence  
- Optional: rollback_module for reversible actions

Example Call  
from interface_module import save

save(file="log.txt", path="/users/current/logs/")

Architectural Notes  
- Interface is the system output layer: model → OS.  
- All actions pass through shaping before being permitted.  
- Modular and extendable per OS (Linux, Windows, macOS).  
- Treated as a high-consequence module: every call is logged and auditable.
