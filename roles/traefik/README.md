# Traefik Role

Deploy Traefik v3.5 reverse proxy and load balancer using Podman containers.

## Requirements

- Podman installed on the target system
- `containers.podman` collection
- Global variable `domain_name` must be defined (used for default service names)

## Role Variables

### Container Configuration

- `traefik_version`: Traefik version to deploy (default: `"v3.5"`)
- `traefik_image`: Container image (default: `"docker.io/traefik:{{ traefik_version }}"`)
- `traefik_container_name`: Container name (default: `"traefik"`)
- `traefik_restart_policy`: Container restart policy (default: `"unless-stopped"`)
- `traefik_log_max_size`: Maximum log file size (default: `"5m"`)

### Network Configuration

- `traefik_network`: Network mode or name (default: `"host"`)
  - Set to `"host"` for host networking
  - Set to any other name to create/use a custom network

### Directory and Volumes

- `traefik_config_dir`: Configuration directory (default: `"/etc/traefik"`)
- `traefik_certs_volume`: Volume name for SSL certificates (default: `"ssl-certs"`)

### Ports

- `traefik_http_port`: HTTP port (default: `80`)
- `traefik_https_port`: HTTPS port (default: `443`)

### Feature Flags

- `traefik_with_http_entrypoint`: Enable HTTP entrypoint (default: `true`)
- `traefik_with_https_redirect`: Redirect HTTP to HTTPS (default: `true`)
- `traefik_with_dashboard`: Enable Traefik dashboard (default: `true`)

### Dashboard Configuration

- `traefik_dashboard_insecure`: Allow insecure dashboard access (default: `false`)
- `traefik_dashboard_service_name`: Dashboard hostname (default: `"dashboard.{{ domain_name }}"`)
- `traefik_dashboard_route_enabled`: Deploy dashboard route config (default: `false`)

### SSL/TLS Configuration

- `traefik_default_cert_resolver`: Default certificate resolver (default: `"letsencrypt"`)
- `traefik_cert_resolvers`: Certificate resolver configurations

### Custom Routes

- `traefik_custom_routes`: List of custom route configurations (default: `[]`)

## Example Playbook

### Basic deployment with host networking:

```yaml
- hosts: servers
  vars:
    domain_name: "example.com"  # Global variable used by all roles
  roles:
    - role: traefik
```

### Custom network deployment:

```yaml
- hosts: servers
  roles:
    - role: traefik
      vars:
        traefik_network: "proxy"
        traefik_dashboard_route_enabled: true
        traefik_dashboard_service_name: "traefik.mydomain.com"
```

### Without HTTP entrypoint:

```yaml
- hosts: servers
  roles:
    - role: traefik
      vars:
        traefik_with_http_entrypoint: false
```

### With custom routes:

```yaml
- hosts: servers
  roles:
    - role: traefik
      vars:
        traefik_custom_routes:
          - name: app
            content: |
              http:
                routers:
                  app:
                    entrypoints: websecure
                    rule: Host(`app.mydomain.com`)
                    service: app
                    tls:
                      certresolver: letsencrypt
                services:
                  app:
                    loadBalancer:
                      servers:
                        - url: http://app-container:8080
```

## License

MIT