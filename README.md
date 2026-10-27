# rhel10_stig — Ansible role (rhel10-stig)

**Ansible hardening role** for **RHEL 10**, generated from the DISA STIG for Red Hat Enterprise Linux 10 profile in ComplianceAsCode SSG 0.1.82. Suitable for playbooks, Packer/Ansible provisioners, and golden-image pipelines. Search keywords: `ansible`, `ansible-role`, `compliance`, `devsecops`, `disa`, `disa-stig`, `hardening`, `infrastructure`, `openscap`, `redhat`, `rhel`, `rhel10`, `security`, `stig`.

StigForge-exported Ansible role **`rhel10_stig`** · release **`0.3.51`**.
Matrix cell status: **`green`**.

## Prefer a ready-made image?

StigReady Applied AMIs for AWS are built with a pinned release of this role, scored with OpenSCAP in the
build, and come with a per-version evidence bundle on request. See [https://stigready.com/applied/](https://stigready.com/applied/).

## Install

This repository root **is** the Ansible role (Galaxy-style layout). Release metadata and the
scoring helper live under `compliance/` and are not loaded when the role runs.

From **GitHub** (public):

```yaml
# requirements.yml
roles:
  - src: https://github.com/stigready/rhel10-stig
    scm: git
    version: v0.3.51   # or an immutable commit SHA
    name: rhel10_stig
```

```bash
ansible-galaxy role install -r requirements.yml -p ./roles
ansible-playbook -i inventory site.yml   # role: rhel10_stig
```

## Verification status (this release)

This release carries no per-role OpenSCAP score, and no OpenSCAP results ship with it. A container
cannot evaluate most hardening rules, so the role is not scored here. To score a host you hardened
with it, see *Score it yourself*. `compliance/releases/0.3.51/manifest.json` records this.

## Exceptions (excluded from the score gate)

These rules are excluded from the OpenSCAP score gate. Most depend on the network or organization the AMI is deployed into, so the role leaves them to the AMI owner; where the role does enforce a control and only the scanner cannot confirm it, the table says so. Each is tracked in the StigForge policy for this role and listed in the image's POA&M. The shared cloud policy strips a further documented set (for example the bootloader password and smartcard rules).

| Rule | Requirement | What this role does | What the AMI owner does |
|---|---|---|---|
| `network_configure_name_resolution` | RHEL 10 STIG (no DISA id in the index yet) | Keeps `dns` in the `hosts:` line of /etc/nsswitch.conf and leaves /etc/resolv.conf as the build network provided it. It never empties the file and never writes a resolver address. | Configure at least two nameservers for your network after launch (a DHCP option set with two DNS servers, or NetworkManager / resolv.conf settings), as the RHEL 10 STIG requires. |
| `selinux_context_elevation_for_sudo` | RHEL 10 STIG (no DISA id in the index yet) | Adds no sudoers line. SSG ships no Ansible remediation for this rule, so the role has nothing to strip; it deliberately does not write the line from the STIG fix text. | Map the administrator login to `sysadm_u` (`semanage login -a -s sysadm_u -r s0-s0:c0.c1023 %wheel`), allow SSH logins for it (`setsebool -P ssh_sysadm_login on`), then add the sudoers line from the STIG. |

## Reports & review

- **[reports/index.html](reports/index.html)** — HTML report index
- **[CHANGELOG.md](CHANGELOG.md)** — release notes

## Score it yourself

This repository ships no OpenSCAP results (see *Verification status*). To score a host you
hardened with this role, run `oscap xccdf eval` on it and score the `results.xml` with this role's
exception list: see **[compliance/README.md](compliance/README.md)**.

## Notices

StigForge and StigReady are independent. They are not affiliated with, endorsed by or certified by the Center for Internet Security, Inc. (CIS), the Defense Information Systems Agency (DISA), the U.S. Department of Defense, NIST, Red Hat or the ComplianceAsCode project. "CIS" and "CIS Benchmarks" are trademarks of the Center for Internet Security, Inc. DISA STIGs are DISA publications. Red Hat and Red Hat Enterprise Linux are trademarks or registered trademarks of Red Hat, Inc. or its subsidiaries in the United States and other countries. Linux is the registered trademark of Linus Torvalds in the U.S. and other countries. All trademarks are the property of their respective owners. This role applies ComplianceAsCode (SSG 0.1.82) remediation from SSG's DISA STIG for Red Hat Enterprise Linux 10 profile. An OpenSCAP score is a measurement, not a certification.

**Login banner.** `dconf_login_banner_contents` and `login_banner_contents` in `defaults/main.yml` default to the DoD Notice and Consent Banner (a U.S. Government information system warning). If the system is not a U.S. Government system, override them with your organization's own text. Overriding the banner text makes the OpenSCAP banner rules fail unless the scan's `login_banner_text` value is tailored to match it or those rules are recorded as exceptions.

## License

Copyright (c) 2026 StigReady.

- **[LICENSE](LICENSE)** (MIT) — StigForge export packaging
- **[NOTICE](NOTICE)** — ComplianceAsCode / BSD-3-Clause task body attribution

## Catalog

- [https://stigready.com/#stigforge](https://stigready.com/#stigforge)

