# Maintenance

Maintenance keeps a Linux system secure, fast, and recoverable. This section
covers updates, cleanup, backups, and recovery.

## Maintenance tasks

| Task | Why it matters | Guide |
|---|---|---|
| Keep the system updated | Installs security fixes and bug fixes | [System maintenance](../guides/system-maintenance.md#keep-the-system-updated) |
| Remove unused software and files | Frees disk space and reduces clutter | [System maintenance](../guides/system-maintenance.md#remove-unused-software) |
| Read logs | Helps identify failing services and hardware | [System maintenance](../guides/system-maintenance.md#read-logs) |
| Back up personal files | Protects documents, photos, and project code | [Backup and restore](./backup-and-restore.md) |
| Create system snapshots | Restores the system after a bad update or driver change | [Backup and restore](./backup-and-restore.md#step-7-system-snapshots-with-timeshift) |
| Check disk health | Detects failing drives before data loss | [System maintenance](../guides/system-maintenance.md#check-disk-health) |
| Recover a broken system | Restores access after a boot failure | [Troubleshooting](../troubleshooting/README.md) |

## A simple maintenance routine

### Weekly

- Install updates.
- Check free disk space.
- Make sure your backups ran.

### Monthly

- Remove unused packages and old kernels using your package manager's official
  cleanup command.
- Check `systemctl --failed` for failed services.
- Test that you can open a file from your backup.

### Before a major change

Create a backup and, if available, a system snapshot before:

- installing or changing graphics drivers
- upgrading to a new distribution release
- repartitioning a disk
- changing boot settings or Secure Boot
- installing a new desktop environment

> **Backups are not optional.** A snapshot is not a backup if it is stored on the
> same failing disk. Keep at least one copy on a separate drive.

## Safety notes

- Do not run cleanup commands you do not understand. Some remove files that are
  still needed.
- Do not delete old kernels manually unless your distribution's documentation
  tells you to.
- Keep the backup drive disconnected or unmounted when it is not in use. This
  reduces the chance that malware or a mistake damages the backup.
- Test a restore before you need it.

## References

- [ArchWiki: System maintenance](https://wiki.archlinux.org/title/System_maintenance)
- [Ubuntu documentation](https://documentation.ubuntu.com/)
- [Fedora documentation](https://docs.fedoraproject.org/)
- [Debian documentation](https://www.debian.org/doc/)
- [openSUSE documentation](https://doc.opensuse.org/)

[Back to documentation home](../README.md) · [Back to project README](../../README.md)
