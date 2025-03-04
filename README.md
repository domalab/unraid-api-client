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

## Using the Unraid API

The Unraid API provides a GraphQL interface that allows you to interact with your Unraid server. This section will help you get started with exploring and using the API.

### Enabling the GraphQL Sandbox

1. First, enable developer mode using the CLI:

    ```bash
    unraid-api developer
    ```

2. Follow the prompts to enable the sandbox. This will allow you to access the Apollo Sandbox interface.

3. Access the GraphQL playground by navigating to:

    ```txt
    http://YOUR_SERVER_IP/graphql
    ```

### Authentication

Most queries and mutations require authentication. You can authenticate using either:

1. API Keys
2. Cookies (default method when signed into the WebGUI)

#### Creating an API Key

Use the CLI to create an API key:

```bash
unraid-api apikey --create
```

Follow the prompts to set:

- Name
- Description
- Roles
- Permissions

The generated API key should be included in your GraphQL requests as a header:

```json
{
    "x-api-key": "YOUR_API_KEY"
}
```

## GraphQL Schema

The Unraid GraphQL API provides access to various aspects of your Unraid server:

### Available Schemas

- Server information (CPU, memory, OS)
- Array status and disk management
- Docker containers and networks
- Network interfaces and shares
- Virtual machines
- User accounts
- Notifications
- API key management
- And more...

### Schema Types

The API includes several core types:

#### Base Types

- `Node`: Interface for objects with unique IDs - please see [Object Identification](https://graphql.org/learn/global-object-identification/)
- `JSON`: For complex JSON data
- `DateTime`: For timestamp values
- `Long`: For 64-bit integers

#### Resource Types

- `Array`: Array and disk management
- `Docker`: Container and network management
- `Info`: System information
- `Config`: Server configuration
- `Connect`: Remote access settings

### Role-Based Access

Available roles:

- `admin`: Full access
- `connect`: Remote access features
- `guest`: Limited read access

## Example Queries

Here are example GraphQL queries you can use:

### System Information

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
            temp
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
        autoStart
        image
    }
}
```

## Best Practices

1. Use the Apollo Sandbox to explore the schema and test queries
2. Start with small queries and gradually add fields as needed
3. Monitor your query complexity to maintain performance
4. Use appropriate roles and permissions for your API keys
5. Keep your API keys secure and rotate them periodically

## Rate Limiting

The API implements rate limiting to prevent abuse. Ensure your applications handle rate limit responses appropriately.

## Error Handling

The API returns standard GraphQL errors in the following format:

```json
{
  "errors": [
    {
      "message": "Error description",
      "locations": [...],
      "path": [...]
    }
  ]
}
```

## Additional Resources

- Use the Apollo Sandbox's schema explorer to browse all available types and fields
- Check the documentation tab in Apollo Sandbox for detailed field descriptions
- Monitor the API's health using `unraid-api status`
- Generate reports using `unraid-api report` for troubleshooting

For more information about specific commands and configuration options, refer to the CLI documentation or run `unraid-api --help`.

## License

This project is open-source software provided as-is.