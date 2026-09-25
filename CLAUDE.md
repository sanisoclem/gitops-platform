# Storage and backups

- `block-transient` (default StorageClass): iSCSI, Delete. Logs, metrics, traces, caches, queues. Not backed up.
- `block-backed`: iSCSI, Retain. The `volsync-block-backed` Kyverno policy gives every such PVC an hourly restic backup to rustfs and makes a new PVC restore from the latest backup. Use it only for state that cannot live on NFS (SQLite, CouchDB).
- App config and libraries go on NFS under `NAS_APPS_PATH`, one directory per app via `subPath`.
- `nas-block` and `nas-file` are legacy; nothing new uses them.

# Postgres

Every CNPG cluster bootstraps with `recovery` from its own backup path and sets `cnpg.io/skipEmptyWalArchiveCheck: enabled`, so a rebuilt cluster restores itself and keeps archiving to the same path. Never run two live clusters against the same `destinationPath` + `serverName`; never change a `serverName`.
