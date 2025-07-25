# App

This role creates and configures an app directory and optionally integrates with Traefik.

## Requirements

None.

## Role Variables

```yaml
# Required
app_name: ""  # Name of the app

# App directory
app_dir: "/srv/apps/{{ app_name }}"  # Default app directory

# Feature flags
app_with_traefik_proxy: false  # Enable Traefik integration

# Traefik hostname (used in Traefik rule)
app_traefik_hostname: "{{ app_name }}"  # Hostname for Traefik routing

# Traefik app URL (backend URL for the app)
app_traefik_url: "http://{{ app_name }}:80"  # Backend app URL

# Traefik configuration (when app_with_traefik_proxy is true)
app_traefik_config: |
  http:
    routers:
      {{ app_name }}:
        entrypoints: websecure
        rule: Host(`{{ app_traefik_hostname }}`)
        service: {{ app_name }}
        tls:
          certresolver: letsencrypt
    services:
      {{ app_name }}:
        loadBalancer:
          servers:
            - url: {{ app_traefik_url }}
```

## Dependencies

When `app_with_traefik_proxy` is enabled, this role expects:
- Traefik to be installed and configured
- The `/etc/traefik/conf.d/` directory to exist

## Example Playbook

```yaml
- hosts: servers
  roles:
    - role: mainstack.app
      vars:
        app_name: myapp
        app_with_traefik_proxy: true
        app_traefik_hostname: api.example.com
        app_traefik_config: |
          http:
            routers:
              myapp:
                entrypoints: websecure
                rule: Host(`api.example.com`)
                service: myapp
                tls:
                  certresolver: letsencrypt
            services:
              myapp:
                loadBalancer:
                  servers:
                    - url: http://localhost:8080
```

## License

MIT

## Author Information

mainstack