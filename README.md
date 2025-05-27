# Ansible Role: update hosts

This Ansible role manages entries in `/etc/hosts` using a clean, dedicated block. It allows you to associate multiple domain names with specific IP addresses and ensures idempotent configuration via `blockinfile`.

## ✅ Features

- Adds multiple domain names per IP address.
- Uses a clearly marked block in `/etc/hosts` (`# BEGIN/END ANSIBLE MANAGED BLOCK`).
- Safe for repeated use (idempotent).
- Preserves unrelated system or manual `/etc/hosts` entries.

## 🧰 Role Variables

Define the IP-to-host mappings in your playbook or inventory:

```yaml
hosts_entries:
  - ip: "127.0.0.1"
    names: ["local.dev", "api.local", "admin.local"]
  - ip: "192.168.1.10"
    names: ["server.local", "db.local"]
````

Each entry must include:

* `ip`: the IP address.
* `names`: a list of hostnames or domain aliases for that IP.

## 📦 Example Usage

### Directory layout

```yaml
your_playbook.yml
roles/
└── manage_hosts/
    ├── tasks/
    │   └── main.yml
    └── defaults/
        └── main.yml
```

### Minimal Playbook

```yaml
- name: Add custom host entries
  hosts: localhost
  become: true
  roles:
    - role: manage_hosts
```

## 📝 Resulting `/etc/hosts` Block

```txt
# BEGIN ANSIBLE MANAGED BLOCK: custom hosts
127.0.0.1 local.dev api.local admin.local
192.168.1.10 server.local db.local
# END ANSIBLE MANAGED BLOCK: custom hosts
```

## 🧪 Testing

Run the role locally using:

```bash
ansible-playbook -i localhost, -c local your_playbook.yml
```

## 🔐 Notes

* The role requires `become: true` to write to `/etc/hosts`.
* Existing entries outside the block are left untouched.

## 🪛 License

MIT

## ✍️ Author

Created by Max Daiber-Huppert with ❤️ via ChatGPT
