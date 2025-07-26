# MyApp Example

This is an example application for testing the mainstack local-app.yaml playbook.

## Usage

From the mainstack root directory, run:

```bash
ansible-playbook playbooks/deploy_simple_containerfile_app.yaml -e app_dir=$PWD/examples/myapp
```

This will:
1. Build a container image from the Containerfile
2. Deploy it to the proxy network
3. Configure Traefik to route traffic to `myapp.<your-hostname>`

The app runs on port 8080 (automatically detected from the EXPOSE directive in the Containerfile).