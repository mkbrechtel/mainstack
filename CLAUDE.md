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

## Release Process

To create a new release:

1. Update the version in `galaxy.yml`
2. Update the CHANGELOG.md with release notes
3. Commit all changes with a descriptive message
4. Create an annotated git tag: `git tag -a v0.0.X -m "Release version 0.0.X - Brief description"`
5. Push commits and tag: `git push origin main --tags`

The GitHub Actions workflow will automatically:
- Build the Ansible collection
- Publish to Ansible Galaxy

Requirements for successful Galaxy import:
- All roles must have a `README.md` file
- All roles must have a `meta/main.yml` file with galaxy_info
- The collection must have a valid `galaxy.yml` file