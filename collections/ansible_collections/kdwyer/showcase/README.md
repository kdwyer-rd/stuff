# kdwyer.showcase

Sample Ansible collection that **ships a role** so you can show why roles
belong *inside* collections instead of sitting as loose folders next to
playbooks.

## Why put roles in collections?

| Loose role in a project | Role inside a collection |
|---|---|
| Referenced as `welcome` (name collisions are easy) | Referenced as **`kdwyer.showcase.welcome`** (FQCN) |
| Versioned only with the whole Git repo | Collection version (`1.0.0`) can be published & pinned |
| Hard to reuse across projects without copy/paste | Install once from Galaxy / Automation Hub / git |
| No standard packaging metadata | `galaxy.yml` documents authors, tags, deps |

Playbooks stay thin: they **import the role** and pass variables. The role owns
the reusable procedure.

## Layout

```text
kdwyer/showcase/
├── galaxy.yml
├── README.md
├── meta/runtime.yml
└── roles/
    └── welcome/          ← the packaged role
        ├── defaults/
        ├── tasks/
        ├── handlers/
        └── meta/
```

## Use the role (FQCN)

```yaml
- hosts: all
  gather_facts: false
  tasks:
    - name: Apply the collection role
      ansible.builtin.import_role:
        name: kdwyer.showcase.welcome
      vars:
        showcase_greeting: "Hello"
        showcase_audience: "AAP"
```

Or with the `roles:` header (still FQCN):

```yaml
- hosts: all
  roles:
    - role: kdwyer.showcase.welcome
      vars:
        showcase_audience: "platform engineers"
```

AAP picks this collection up automatically from the project path  
`collections/ansible_collections/` when you sync the `stuff` project.

## Role variables

See [`roles/welcome/README.md`](roles/welcome/README.md).
