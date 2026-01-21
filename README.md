# macOS Self Service+ Preparation (Jamf)

This repository documents **my practical approach** to preparing Macs to use the new **Self Service+** while keeping a safe and fast rollback path. It intentionally avoids deleting the classic **Self Service.app**. Instead, I hide it and (optionally) remove any Dock items that point to it.

> **Why not delete Classic Self Service?**  
> Deleting can force full re-enrollment to restore, which is too risky and slow if I need to roll back. Hiding is safer and instantly reversible.

## What this repo includes
- Shell scripts to:
  - **Hide** classic Self Service by renaming the app bundle (dot-prefix)
  - **Unhide** the app (rollback)
  - **Remove** classic Self Service from the Dock for all users (requires `dockutil`)
- Basic guidance on using these in Jamf or locally

---

## Scripts

### 1) Hide classic Self Service
```bash
# hide_self_service.sh
mv "/Applications/Self Service.app" "/Applications/.Self Service.app"
```

### 2) Unhide (rollback)
```bash
# unhide_self_service.sh
mv "/Applications/.Self Service.app" "/Applications/Self Service.app"
```

### 3) Remove classic Self Service from the Dock (all user homes)
> **Requires** the `dockutil` binary to be present at `/usr/local/bin/dockutil`.
```bash
# remove_self_service_from_dock.sh
#!/bin/bash
/usr/local/bin/dockutil --remove 'Self Service' --allhomes
```

---

## Usage Notes

- **Order of operations (suggested):**
  1. Hide classic Self Service
  2. Remove it from the Dock (if present)
  3. Deploy/enable Self Service+

- **Rollback:**
  - Run the unhide script to instantly restore classic **Self Service.app** in `/Applications`.
  - If you previously removed the Dock item, you can re-add as needed using your normal Dock management approach.

- **Jamf deployment tips (from my experience):**
  - Use separate policies (or scripts) for **hide**, **dock removal**, and **unhide** so you can toggle or scope quickly.
  - If you *must* run Dock changes, ensure `dockutil` is installed **before** the Dock script executes (e.g., as a package or prerequisite policy).
  - Avoid deleting the app bundle—renaming is safer and preserves a clean, fast rollback path without re-enrollment.

---

## Repository Structure
```
macos-self-service-plus-prep/
├─ README.md
├─ scripts/
│  ├─ hide_self_service.sh
│  ├─ unhide_self_service.sh
│  └─ remove_self_service_from_dock.sh
├─ .gitignore
├─ LICENSE (MIT)
└─ CHANGELOG.md
```

---

## License
MIT — see [LICENSE](LICENSE).

