# Changelog

Format based on [Keep a Changelog](https://keepachangelog.com/).

## [0.2.1-private-review] - export review

### Added
- Initial StigForge export of matrix role `rhel10_stig`.
- OpenSCAP verify evidence bundles per profile under `compliance/releases/`.

### Verified (CI)

- **`stig`** — score **88.57%** (floor 90.0%) · gate **FAIL** · evidence `20260726T235430Z`
  - OpenSCAP failures still counted: `accounts_umask_etc_bashrc, accounts_umask_etc_profile, configure_crypto_policy, ensure_gpgcheck_local_packages, file_permission_user_init_files_root, network_configure_name_resolution, rootfiles_configured, use_pam_wheel_for_su`

### Provenance

- Factory pipeline: https://github.com/stigready/stigforge/actions/runs/30277229616
- Factory commit: `ccc5f55bd24b1c9cb1b8ca95a70b55443e0064b5`

