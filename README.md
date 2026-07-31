# stuff

Sample Ansible content for AAP / Automation Portal demos.

| Path | Purpose |
|------|---------|
| `hello_world.yml` | Minimal debug play |
| `restart_service.yml` | Survey-driven systemd service restart |
| `survey_hello.yml` | Survey hello demo (no become; safe on localhost) |
| `patching_linux.yml` | Role-based patching sample |
| `playbooks/api/` | **Showcase: call the Controller API to list/launch plays** |
| `collections/ansible_collections/kdwyer/showcase/` | Sample collection + `welcome` role (FQCN demo) |
| `use_showcase_role.yml` | Thin playbook that imports `kdwyer.showcase.welcome` |
| `rulebooks/` | EDA rulebook samples |
| `catalog-info.yaml` | Backstage / Automation Portal catalog entity |

See [`playbooks/api/README.md`](playbooks/api/README.md) for API demo usage.
