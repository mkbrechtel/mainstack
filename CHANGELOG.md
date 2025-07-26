# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- `deploy_simple_containerfile_app.yaml` playbook for deploying local containerized applications
  - Automatically builds container images from local Containerfiles
  - Uses directory name as application name
  - Automatically detects exposed port from Containerfile EXPOSE directive (defaults to 80)
  - Integrates with Traefik proxy using file-based configuration
  - Creates Traefik routing configuration at `/etc/traefik/conf.d/{{ app_name }}.yaml`
  - Uses `ansible_fqdn` for domain name configuration
  - Supports custom port override via `container_port` variable
- Example application in `examples/myapp/` demonstrating the playbook usage
  - Nginx-based container exposing port 8080
  - Custom nginx configuration and static HTML content
  - README with usage instructions

## [0.0.2] - 2025-07-25

### Added
- Podman role for installing container runtime with DNS support
  - Installs podman, dnsmasq, containernetworking-plugins, and podman-compose
  - Includes dnsname plugin for container DNS resolution
  - Daemonless operation (no systemd service management)
- System playbook (`system.yaml`) for base system setup
  - Configures Podman installation on target hosts
  - Uses local connection by default for localhost deployment
- App role for managing app directories and Traefik integration
  - Configurable app directory (defaults to `/srv/apps/{{ app_name }}`)
  - Optional Traefik proxy integration with `app_with_traefik_proxy` feature flag
  - Separate `app_traefik_hostname` variable for Traefik routing (defaults to `app_name`)
  - Explicit `app_traefik_url` variable for backend app URL configuration
  - Customizable Traefik configuration through `app_traefik_config` variable
  - Automatic Traefik configuration file generation at `/etc/traefik/conf.d/{{ app_name }}.yaml`
- Whoami role as example implementation using the app role
  - Deploys traefik/whoami container for testing HTTP routing
  - Demonstrates app role usage with Traefik integration
  - Configurable port mapping and container settings
  - Support for multiple instances with different configurations

### Changed
- Whoami role container naming convention updated
  - Container name now uses full `app_name` with `app_internal_domain` suffix
  - Removed exposed port mapping as container uses internal proxy network
- All roles now use a global "proxy" network for inter-container communication
  - Traefik role: Changed default network from "host" to "proxy"
  - App role: Added `app_network` configuration defaulting to "proxy"
  - Whoami role: Changed default network from "podman" to "proxy"
- Implemented templated internal domain naming convention
  - Added `app_internal_domain` variable (defaults to "app.internal") in app and whoami roles
  - Services now use `{{ app_name }}.{{ app_internal_domain }}` for internal communication
  - Container hostnames follow the same naming pattern for DNS resolution

### Removed
- Removed `traefik_custom_routes` feature from Traefik role
  - Use individual app roles with Traefik integration instead

### Documentation
- Added "_with_" feature flag pattern documentation to CLAUDE.md
- Documented the convention for optional functionality in roles
- Added comprehensive proxy network architecture documentation to CODING.md

## [0.0.1] - 2025-07-25

### Added
- Traefik role for deploying Traefik v3.5 reverse proxy with Podman
  - Configurable network mode (defaults to host networking)
  - Feature flags for HTTP entrypoint, HTTPS redirect, and dashboard
  - Support for custom route configurations
  - Let's Encrypt integration with production and staging resolvers
  - Global variable `domain_name` support for service naming
  - Dashboard route configuration at `traefik.{{ domain_name }}`
- Local playbook (`local.yaml`) for deploying to localhost with automatic domain detection

## [0.0.0-dev] - 2025-07-25

### Added
- Initial development release of mkbrechtel.mainstack Ansible collection
- Base components for containerized web applications
- Support for Podman containerization framework
- Core infrastructure roles for web application deployment
- Systemd integration for container management
- Comprehensive documentation and examples

### Dependencies
- containers.podman: >=1.10.0
- ansible.posix: >=1.5.0

[0.0.2]: https://github.com/mkbrechtel/mainstack/compare/v0.0.1...v0.0.2
[0.0.1]: https://github.com/mkbrechtel/mainstack/compare/v0.0.0-dev...v0.0.1
[0.0.0-dev]: https://github.com/mkbrechtel/mainstack/releases/tag/v0.0.0-dev