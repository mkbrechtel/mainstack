## Ansible Project Structure

### Global Variables

This collection uses global variables that are shared across all roles:

- `domain_name`: The primary domain name used throughout the collection. This variable is used by roles to construct service-specific subdomains (e.g., `dashboard.{{ domain_name }}` for Traefik dashboard).

### File Organization

Files can be placed in `./files/` at the collection root instead of in individual role directories. Ansible's search path includes the collection's files directory, so roles can reference these files directly without duplication.

### Feature Flags Pattern

This collection uses a "_with_" naming convention for optional feature flags in roles. These boolean variables enable or disable specific functionality within a role:

- Feature flags follow the pattern: `<role_name>_with_<feature>`
- Examples: `app_with_traefik_proxy`, `traefik_with_dashboard`, `traefik_with_https_redirect`
- This pattern allows roles to have a core functionality with optional extensions
- Feature flags should default to `false` to maintain backward compatibility

## Network Architecture

### Proxy Network Pattern

All containerized applications in this collection use a shared "proxy" network for inter-container communication. This design pattern ensures:

1. **No Direct Port Exposure**: Application containers should NOT expose ports directly to the host. Instead, they rely on Traefik (or another reverse proxy) to handle external access.

2. **Internal Communication**: Containers communicate with each other using internal hostnames following the pattern: `{{ app_name }}.{{ app_internal_domain }}` (e.g., `whoami.example.com.app.internal`)

3. **Container Naming**: Container names should include the app_internal_domain suffix for consistency: `{{ app_name }}.{{ app_internal_domain }}`

Example implementation:
```yaml
- name: Deploy application container
  containers.podman.podman_container:
    name: "{{ app_name }}.{{ app_internal_domain }}"
    network: "proxy"  # Use the shared proxy network
    hostname: "{{ app_name }}.{{ app_internal_domain }}"
    # No ports mapping - external access via Traefik only
```

This approach provides:
- Better security by limiting exposed attack surface
- Simplified networking configuration
- Consistent service discovery within the proxy network
- Centralized routing and SSL termination through Traefik

