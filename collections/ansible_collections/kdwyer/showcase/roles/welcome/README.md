# Role: kdwyer.showcase.welcome

Reusable welcome role **shipped inside** the `kdwyer.showcase` collection.

## Entry point

```yaml
- ansible.builtin.import_role:
    name: kdwyer.showcase.welcome
```

## Variables

| Variable | Default | Purpose |
|---|---|---|
| `showcase_greeting` | `Welcome` | Leading greeting word |
| `showcase_audience` | `world` | Who is greeted |
| `showcase_write_file` | `true` | Write a marker file |
| `showcase_output_path` | `/tmp/kdwyer-showcase-welcome.txt` | Marker path |

## Teaching point

System depend on **`kdwyer.showcase`**, not a random `roles/welcome` folder.
That is how you share, version, and avoid role-name collisions in AAP.
