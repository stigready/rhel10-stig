# rhel10_stig — Ansible role (rhel10-stig)

**Ansible hardening role** for **RHEL 10** (DISA STIG). Suitable for playbooks, Packer/Ansible provisioners, and golden-image pipelines. Search keywords: `ansible`, `ansible-role`, `compliance`, `devsecops`, `disa`, `disa-stig`, `hardening`, `infrastructure`, `openscap`, `redhat`, `rhel`, `rhel10`, `security`, `stig`.

StigForge-exported Ansible role **`rhel10_stig`** · release **`0.3.49`**.
Matrix cell status: **`green`**.

## Install

This repository root **is** the Ansible role (Galaxy-style layout). OpenSCAP evidence
lives under `compliance/` and is not loaded when the role runs.

From **GitHub** (public):

```yaml
# requirements.yml
roles:
  - src: https://github.com/stigready/rhel10-stig
    scm: git
    version: v0.3.49   # or an immutable commit SHA
    name: rhel10_stig
```

```bash
ansible-galaxy role install -r requirements.yml -p ./roles
ansible-playbook -i inventory site.yml   # role: rhel10_stig
```

## Verification status (this release)

This release carries no per-role OpenSCAP score. A container cannot evaluate most STIG rules, so the
role is scored where it runs: the StigReady AMI build scores each image against the unmodified SSG
profile. `compliance/releases/0.3.49/manifest.json` records this.

## Exceptions (excluded from the score gate)

These rules are excluded from the OpenSCAP score gate. Most depend on the network or organisation the AMI is deployed into, so the role leaves them to the AMI owner; where the role does enforce a control and only the scanner cannot confirm it, the table says so. Each is tracked in the StigForge policy for this role and listed in the image's POA&M. The shared cloud policy strips a further documented set (for example the bootloader password and smartcard rules).

| Rule | Requirement | What this role does | What the AMI owner does |
|---|---|---|---|
| `network_configure_name_resolution` | RHEL 10 STIG (no DISA id in the index yet) | Keeps `dns` in the `hosts:` line of /etc/nsswitch.conf and leaves /etc/resolv.conf as the build network provided it. It never empties the file and never writes a resolver address. | Configure at least two nameservers for your network after launch (a DHCP option set with two DNS servers, or NetworkManager / resolv.conf settings), as the RHEL 10 STIG requires. |
| `selinux_context_elevation_for_sudo` | RHEL 10 STIG (no DISA id in the index yet) | Adds no sudoers line. SSG ships no Ansible remediation for this rule, so the role has nothing to strip; it deliberately does not write the line from the STIG fix text. | Map the administrator login to `sysadm_u` (`semanage login -a -s sysadm_u -r s0-s0:c0.c1023 %wheel`), allow SSH logins for it (`setsebool -P ssh_sysadm_login on`), then add the sudoers line from the STIG. |

## Reports & review

- **[reports/index.html](reports/index.html)** — HTML report index
- **[CHANGELOG.md](CHANGELOG.md)** — release notes and verify summary

## Verify the score (customer)

Re-run OpenSCAP in Docker and compare to this release's evidence:

```bash
make prove RELEASE=0.3.49
```

Or score your own `results.xml`: see **[compliance/README.md](compliance/README.md)**.

## License

- **[LICENSE](LICENSE)** (MIT) — StigForge export packaging
- **[NOTICE](NOTICE)** — ComplianceAsCode / BSD-3-Clause task body attribution

## Catalog

- [https://stigready.com/#stigforge](https://stigready.com/#stigforge)

