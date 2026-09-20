# backup-cli

Personal shell tool for synchronizing `~/SYNC` with a remote `SHARE` directory
over SSH/rsync. It selects a local or remote endpoint by pinging both addresses,
can mount the share through SSHFS, and uses Rofi to confirm destructive syncs.

> [!CAUTION]
> This is workstation-specific software, not a general backup solution. The
> remote SSH user is hard-coded as `gabriel`, several commands run through
> `sudo`, connectivity detection parses French `ping` output, and `-d` enables
> deletion. Read `src/backup-cli` before using it with real data.

## Build

```bash
nix build github:RevolunixOS/pkg-backup-cli
```

The package exposes the `backup-cli` executable. The current wrapper only adds
Rofi to `PATH`; the host must also provide SSH, SSHFS, rsync, ping, mountpoint,
and a working SSH key.

## Configuration

The script sources `~/.config/backup-cli.sh` and expects at least:

```bash
LOCAL_IP="192.168.1.10"
NET_DOMAINE="example.net"
```

It uses `~/.ssh/id_ed25519`, the local directories `~/SYNC` and `~/SHARE`, and
the remote relative path `./SHARE/`.

## Commands

```text
backup-cli -m          mount the remote share
backup-cli -p          pull remote files into ~/SYNC
backup-cli -P          push ~/SYNC to the remote share
backup-cli -s          push, then pull
backup-cli -sl         repeat synchronization every 30 seconds
backup-cli <cmd> -d    propagate deletions
backup-cli <cmd> -sd   preview and confirm deletions through Rofi
```

Test with disposable directories and without `-d` first. This tool does not
provide versioned snapshots, encryption at rest, or protection against a bad
sync propagating deletions.

## License

See [`LICENSE`](LICENSE).
