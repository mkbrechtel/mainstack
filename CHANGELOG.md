# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Service role for managing service directories and Traefik integration
  - Configurable service directory (defaults to `/srv/{{ service_name }}`)
  - Optional Traefik proxy integration with `service_with_traefik_proxy` feature flag
  - Separate `service_traefik_hostname` variable for Traefik routing (defaults to `service_name`)
  - Customizable Traefik configuration through `service_traefik_config` variable
  - Automatic Traefik configuration file generation at `/etc/traefik/conf.d/{{ service_name }}.yaml`

### Documentation
- Added "_with_" feature flag pattern documentation to CLAUDE.md
- Documented the convention for optional functionality in roles

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

[0.0.1]: https://github.com/mkbrechtel/mainstack/compare/v0.0.0-dev...v0.0.1
[0.0.0-dev]: https://github.com/mkbrechtel/mainstack/releases/tag/v0.0.0-dev