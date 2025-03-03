# Unraid GraphQL API Client

This repository contains tools and scripts for testing and querying the Unraid GraphQL API. It includes both a Python client and a shell script for different usage scenarios.

## Getting Started

### Prerequisites

- Python 3.6+
- Required Python packages (install with `pip install -r requirements.txt`)
- For shell script usage: bash, curl, and optionally jq for pretty-printing JSON

### Installation

1. Clone or download this repository
2. Install required dependencies:

```bash
pip install -r requirements.txt
```

## Usage

### Python Client

The Python client (`unraid_api_client.py`) provides a comprehensive interface for working with the Unraid GraphQL API.

**Basic usage:**

```bash
# Query basic server information
python3 unraid_api_client.py --ip 192.168.20.21 --key YOUR_API_KEY

# Query array status
python3 unraid_api_client.py --ip 192.168.20.21 --key YOUR_API_KEY --query array

# Run multiple queries
python3 unraid_api_client.py --ip 192.168.20.21 --key YOUR_API_KEY --query all
```

**Available query types:**
- `info`: Basic server information
- `versions`: Version information for OS, kernel, etc.
- `array`: Array status overview
- `array-disks`: Detailed array disk information
- `parity`: Parity check status
- `docker`: Docker containers
- `docker-networks`: Docker networks
- `disks`: Physical disks information
- `network`: Network interfaces
- `shares`: Network shares
- `vms`: Virtual machines
- `users`: User accounts
- `notifications`: System notifications
- `api-keys`: API keys information
- `flash`: Flash storage information
- `all`: Run all queries

**Additional options:**
- `--https`: Use HTTPS instead of HTTP
- `--direct`: Skip redirect detection and connect directly to the IP
- `--custom "query { ... }"`: Run a custom GraphQL query
- `--important-only`: When querying notifications, only show important ones

### Shell Script

The shell script (`test_api_curl.sh`) provides a simpler alternative using curl:

```bash
# Query basic server information
./test_api_curl.sh --ip 192.168.20.21 --key YOUR_API_KEY

# Query docker containers
./test_api_curl.sh --ip 192.168.20.21 --key YOUR_API_KEY --type docker

# Use HTTPS
./test_api_curl.sh --ip 192.168.20.21 --key YOUR_API_KEY --https --type network
```

## GraphQL Schema

The Unraid GraphQL API provides access to various aspects of your server:

- Server information (CPU, memory, OS)
- Array status and disk management
- Docker containers and networks
- Network interfaces and shares
- Virtual machines
- User accounts
- Notifications
- API key management
- And more...

## Authentication

Authentication is handled via API keys. Create an API key in the Unraid WebUI or via CLI:

```bash
unraid-api apikey --create
```

Include the API key in your requests as a header:

```
x-api-key: YOUR_API_KEY
```

## Example Queries

Here are a few example GraphQL queries you can use:

### Server Information

```graphql
query {
  info {
    os {
      platform
      distro
      release
      uptime
    }
    cpu {
      manufacturer
      brand
      cores
      threads
    }
    memory {
      total
      free
      used
    }
  }
}
```

### Array Status

```graphql
query {
  array {
    state
    capacity {
      disks {
        free
        used
        total
      }
    }
    disks {
      name
      size
      status
    }
  }
}
```

### Docker Containers

```graphql
query {
  dockerContainers {
    id
    names
    state
    status
    image
  }
}
```

## License

This project is open-source software provided as-is.