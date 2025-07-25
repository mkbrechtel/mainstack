# Whoami

This role deploys the traefik/whoami container service, which is useful for testing HTTP routing and load balancing configurations. It demonstrates how to use the service role for setting up a containerized service with Traefik integration.

## Requirements

- Podman installed on the target host
- Service role (uses service role as a dependency)
- Traefik deployed (when using Traefik integration)

## Role Variables

```yaml
# Container configuration
whoami_image: "docker.io/traefik/whoami:latest"
whoami_container_name: "whoami"
whoami_port: 8080
whoami_restart_policy: "unless-stopped"

# Service configuration (passed to service role)
whoami_service_name: "whoami"
whoami_with_traefik: false
whoami_traefik_hostname: "{{ whoami_service_name }}"

# The service role will automatically configure Traefik routing
# using service_traefik_url which defaults to http://localhost:{{ whoami_port }}
```

## Dependencies

This role depends on:
- service

## Example Playbook

Basic deployment without Traefik:
```yaml
- hosts: servers
  roles:
    - role: mkbrechtel.mainstack.whoami
```

With Traefik integration:
```yaml
- hosts: servers
  roles:
    - role: mkbrechtel.mainstack.whoami
      vars:
        whoami_with_traefik: true
        whoami_traefik_hostname: whoami.example.com
```

Multiple whoami instances:
```yaml
- hosts: servers
  tasks:
    - name: Deploy first whoami instance
      include_role:
        name: mkbrechtel.mainstack.whoami
      vars:
        whoami_service_name: whoami1
        whoami_container_name: whoami1
        whoami_port: 8081
        whoami_with_traefik: true
        whoami_traefik_hostname: whoami1.example.com

    - name: Deploy second whoami instance
      include_role:
        name: mkbrechtel.mainstack.whoami
      vars:
        whoami_service_name: whoami2
        whoami_container_name: whoami2
        whoami_port: 8082
        whoami_with_traefik: true
        whoami_traefik_hostname: whoami2.example.com
```

## What Whoami Does

The whoami service is a simple HTTP server that responds with information about the HTTP request it receives, including:
- Hostname (container ID)
- IP addresses
- HTTP headers
- Request URI
- HTTP method
- Remote address

This makes it perfect for:
- Testing load balancer configurations
- Debugging HTTP routing rules
- Verifying proxy headers
- Testing TLS termination

## License

MIT

## Author Information

mainstack