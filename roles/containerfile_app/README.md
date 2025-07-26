# Containerfile App Role

Deploy containerized applications from local Containerfiles with automatic Traefik integration.

## Features

- Builds container images from local Containerfiles
- Automatically detects exposed ports from EXPOSE directives
- Integrates with Traefik reverse proxy
- Supports custom naming and configuration

## Variables

### Required Variables

- `containerfile_app_directory`: Path to directory containing the Containerfile (required)
- `domain_name`: Base domain for the application (global variable)

### Optional Variables

- `containerfile_app_name`: Override the application name (defaults to directory basename)
- `containerfile_app_default_port`: Default port if none detected (default: 80)
- `containerfile_app_force_rebuild`: Force rebuild of container image (default: true)
- `containerfile_app_network`: Container network (default: proxy)
- `containerfile_app_with_traefik`: Enable Traefik integration (default: true)
- `containerfile_app_traefik_entrypoint`: Traefik entrypoint (default: websecure)
- `containerfile_app_traefik_certresolver`: Certificate resolver (default: letsencrypt)

## Example Usage

```yaml
- name: Deploy my application
  hosts: localhost
  roles:
    - role: containerfile_app
      vars:
        containerfile_app_directory: /path/to/myapp
        domain_name: example.com
```

This will:
1. Build a container from `/path/to/myapp/Containerfile`
2. Deploy it as `myapp.example.com.app.internal`
3. Configure Traefik to route `myapp.example.com` to the container