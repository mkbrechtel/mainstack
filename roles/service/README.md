# Service

This role creates and configures a service directory and optionally integrates with Traefik.

## Requirements

None.

## Role Variables

```yaml
# Required
service_name: ""  # Name of the service

# Service directory
service_dir: "/srv/{{ service_name }}"  # Default service directory

# Feature flags
service_with_traefik_proxy: false  # Enable Traefik integration

# Traefik hostname (used in Traefik rule)
service_traefik_hostname: "{{ service_name }}"  # Hostname for Traefik routing

# Traefik configuration (when service_with_traefik_proxy is true)
service_traefik_config: |
  http:
    routers:
      {{ service_name }}:
        entrypoints: websecure
        rule: Host(`{{ service_traefik_hostname }}`)
        service: {{ service_name }}
        tls:
          certresolver: letsencrypt
    services:
      {{ service_name }}:
        loadBalancer:
          servers:
            - url: http://{{ service_name }}:80
```

## Dependencies

When `service_with_traefik_proxy` is enabled, this role expects:
- Traefik to be installed and configured
- The `/etc/traefik/conf.d/` directory to exist

## Example Playbook

```yaml
- hosts: servers
  roles:
    - role: mainstack.service
      vars:
        service_name: myapp
        service_with_traefik_proxy: true
        service_traefik_hostname: api.example.com
        service_traefik_config: |
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