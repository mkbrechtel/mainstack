# mainstack Ansible Collection

**mainstack** provides base components for containerized web applications. We aim to support multiple containerization frameworks, starting with Podman.

> **⚠️ Development Phase Notice**  
> This collection is currently in development (version 0.x.x). Breaking changes may occur in any release until we reach version 1.0.0. APIs, role interfaces, and variable names are subject to change.

## Installation

```bash
ansible-galaxy collection install mkbrechtel.mainstack
```

## Requirements

- Ansible >= 2.14.3
- Podman (for container management)

## Global Variables

This collection uses global variables that are shared across all roles:

- `domain_name`: The primary domain name used throughout the collection. Roles use this to construct service-specific subdomains (e.g., `dashboard.{{ domain_name }}` for Traefik dashboard)

## Usage

[To be filled with examples as roles are developed]

## License

Apache-2.0
