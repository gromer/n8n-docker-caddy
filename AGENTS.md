# Repository guidance

## Purpose and ownership

This repository owns the Docker Compose definition and Caddy reverse-proxy configuration for a self-hosted n8n deployment. Compose connects Caddy to n8n on its private network, publishes Caddy on ports 80 and 443, and declares external `caddy_data` and `n8n_data` volumes. The host `local_files/` directory is mounted in n8n at `/files`.

This repository does not provision or administer the host, cloud account, DNS, firewall, or other external network policy. It also does not own n8n application code or workflows. Make changes to those systems in their owning repositories or operational systems. The [README](README.md) links to the supported DigitalOcean and Hetzner setup guides; consult those for host setup details.

## Change boundaries and sensitive data

- Preserve the private n8n service boundary: the Compose file does not publish port 5678 on the host. Changes to published ports, proxy routing, or Caddy hostnames alter public access and should stay consistent across `docker-compose.yml`, `caddy_config/Caddyfile`, and deployment documentation.
- Treat `n8n_data` and `caddy_data` as persistent state. This repository declares these volumes as external; it does not create, back up, migrate, or clean them up. Do not change their lifecycle or assume that `docker compose down` manages their contents.
- `.env` is tracked and supplies deployment substitutions. Do not retrieve or display its values, or values from rendered Compose configuration, container environment/inspection output, or credentials in logs and diagnostics. Prefer variable names, file metadata, and quiet validation. Do not retrieve private key material or credential-bearing Caddy state.
- Keep host provisioning, DNS, firewall, and cloud resource changes out of this repository unless its checked-in deployment model is explicitly expanded to own them.

## Validation and workflow

- For Compose edits, run `docker compose config --quiet` from the repository root. Do not print the rendered configuration.
- For documentation-only changes, review referenced repository paths and run `git diff --check`. There are no checked-in automated tests, CI workflows, or Markdown link checker.
- Do not start, deploy, or alter a running stack solely to validate a repository change. Runtime checks for routing or persistence changes belong in a suitable deployment environment and are not a prerequisite for documentation-only work.
- Use a focused branch, validate, commit, push, and create or update a focused PR against the default branch. Report validation in the PR. Do not merge unless explicitly requested.
