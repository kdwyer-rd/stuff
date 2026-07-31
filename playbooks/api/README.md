# API demos — call Ansible Automation Platform to list / launch plays
#
# These playbooks talk to the Controller API (`/api/controller/v2/`) so you can
# drive job templates (and ad-hoc modules) from Ansible itself.
#
# | Playbook | What it shows |
# |---|---|
# | `list_job_templates.yml` | `GET /me/` + `GET /job_templates/` with `ansible.builtin.uri` |
# | `launch_job_uri.yml` | Look up JT → `POST .../launch/` → poll job → stdout |
# | `launch_job_controller.yml` | Same flow with `ansible.controller.job_launch` / `job_wait` |
# | `launch_adhoc_uri.yml` | `POST /ad_hoc_commands/` for one-shot modules (`ping`, etc.) |
#
# ## One-time setup
#
# ```bash
# cd playbooks/api
# cp vars.example.yml vars.yml   # edit aap_host + token or user/password
# # optional, for launch_job_controller.yml:
# ansible-galaxy collection install -r collections-requirements.yml
# ```
#
# Create an AAP token: Access Management → Users → (you) → Tokens
# (or Applications / OAuth). Prefer token over password.
#
# ## Run from your laptop
#
# ```bash
# # Who am I + list templates this credential can see
# ansible-playbook -i inventory list_job_templates.yml
#
# # Launch an existing job template (e.g. stuff-hello_world) and wait
# ansible-playbook -i inventory launch_job_uri.yml \
#   -e job_template_name=stuff-hello_world
#
# # Same launch using the ansible.controller collection
# ansible-playbook -i inventory launch_job_controller.yml \
#   -e job_template_name=stuff-hello_world
#
# # Pass extra_vars into the launched play
# ansible-playbook -i inventory launch_job_uri.yml \
#   -e job_template_name=stuff-hello_world \
#   -e '{"job_extra_vars":{"target_env":"dev"}}'
# ```
#
# ## Equivalent curl (launch)
#
# ```bash
# AAP=https://example-aap.apps.example.com
# TOKEN=...
# JT=13   # job template id
#
# curl -sk -H "Authorization: Bearer $TOKEN" -H 'Content-Type: application/json' \
#   -X POST "$AAP/api/controller/v2/job_templates/$JT/launch/" \
#   -d '{"extra_vars":{}}'
# ```
#
# ## Run inside AAP
#
# 1. Sync the `stuff` project so these playbooks appear.
# 2. Create job templates pointing at e.g. `playbooks/api/launch_job_uri.yml`
#    with inventory "localhost" (or Demo Inventory) and prompt-on-launch for
#    `aap_host`, `aap_token`, `job_template_name` — or store the token in a
#    Controller credential / custom credential type.
# 3. Important: do **not** launch a JT that launches itself without a guard,
#    or you can recurse forever. Point the API demos at a *different* JT
#    (hello_world, RFE-sleeper, …).
#
# ## Auth notes (AAP 2.5+)
#
# - Base URL is the gateway route; Controller paths stay under `/api/controller/v2/`.
# - Token: `Authorization: Bearer <token>`
# - Or HTTP basic with a local user (fine for labs; prefer tokens elsewhere).
