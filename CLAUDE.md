# CLAUDE.md - Development Guidelines

## Build & Run Commands
- Run client with default settings: `python3 unraid_api_client.py`
- Query server info: `python3 unraid_api_client.py --query [info|array|docker|disks|network|shares|vms|parity|vars|users|apikeys|notifications|all]`
- Run with custom GraphQL query: `python3 unraid_api_client.py --custom "{ info { os { platform } } }"`
- Test API with curl script: `./test_api_curl.sh --type [info|array|docker|disks|network|shares|vms]`
- Install dependencies: `pip install -r requirements.txt`

## System Control Commands
- Reboot Unraid: `python3 unraid_api_client.py --reboot`
- Shutdown Unraid: `python3 unraid_api_client.py --shutdown`
- Start Array: `python3 unraid_api_client.py --start-array`
- Stop Array: `python3 unraid_api_client.py --stop-array`
- Parity Check: `python3 unraid_api_client.py --start-parity`
- Parity Check with Correction: `python3 unraid_api_client.py --correct-parity`
- Pause/Resume/Cancel Parity Check: `--pause-parity`, `--resume-parity`, `--cancel-parity`

## Docker Container & VM Control
**Note:** The current Unraid GraphQL API does not support direct control of Docker containers or VMs.
The client includes placeholder methods for these operations for future API compatibility.

The following operations are provided for information but are not currently functional:
- Start Container: `python3 unraid_api_client.py --start-container CONTAINER_ID`
- Stop Container: `python3 unraid_api_client.py --stop-container CONTAINER_ID`
- Restart Container: `python3 unraid_api_client.py --restart-container CONTAINER_ID`
- Start VM: `python3 unraid_api_client.py --start-vm VM_UUID`
- Stop VM: `python3 unraid_api_client.py --stop-vm VM_UUID`
- Pause VM: `python3 unraid_api_client.py --pause-vm VM_UUID`
- Resume VM: `python3 unraid_api_client.py --resume-vm VM_UUID`

To manage Docker containers and VMs, please use the Unraid web interface or other Unraid tools.

## User Management
- Add User: `python3 unraid_api_client.py --add-user --username name --password pass --description "desc"`
- Delete User: `python3 unraid_api_client.py --delete-user --username name`
- View Current User: `python3 unraid_api_client.py --query users`

## API Key Management
- Create API Key: `python3 unraid_api_client.py --create-apikey --apikey-name name --description "desc" --apikey-roles "admin,guest"`
- List API Keys: `python3 unraid_api_client.py --query apikeys`

## Notification Management
- Create Notification: `python3 unraid_api_client.py --create-notification --title "Title" --subject "Subject" --message "Message" --importance INFO`
- Archive Notification: `python3 unraid_api_client.py --archive-notification notification_id`
- Archive All Notifications: `python3 unraid_api_client.py --archive-all`
- List Notifications: `python3 unraid_api_client.py --query notifications`

## Remote Access
- Setup Remote Access: `python3 unraid_api_client.py --setup-remote --access-type DYNAMIC --forward-type UPNP --remote-port 443`

## Code Style Guidelines
- **Imports**: Standard library first, then third-party, then local modules (grouped with line breaks)
- **Formatting**: 4-space indentation, max 100 chars per line, docstrings for classes and methods
- **Types**: Use type hints via Python's typing module (Dict, Any, Optional, etc.)
- **Naming**: 
  - Classes: CamelCase
  - Methods/Functions: snake_case
  - Constants: UPPER_CASE
- **Error Handling**: Use try/except blocks with specific exceptions, log error details
- **Documentation**: Maintain docstrings with Args/Returns sections formatted for clarity
- **GraphQL**: Format GraphQL queries with consistent indentation inside triple-quoted strings

## Project Structure
- Client class follows single-responsibility principle with focused methods for each query type
- Command-line interface with argparse for flexible usage
- Handles authentication via API key and manages redirects automatically