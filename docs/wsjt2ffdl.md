# WSJT2FFDL
This utility listens for multicasted QSO log events from WSJT-X
and publishes them to the [Firefly Field Day Logger](https://github.com/jxmx/ffdl).
This requires WSJT-X to be configured to send to a multicast IP
as described in [Setup WSJT-X](wsjtx.md).

## Installation
Installation is supported only on the Firefly Logger Pi Appliance image
and supported versions of Debian (currently Debian 12 Bookworm) only
and only when installed through use of apt/deb and the PacketWarriors
software repository. 

### Appliance Image
wsjt2ffdl is installed and loaded on the appliance image. No 
further installation or configuration is necessary.

### Debian Installation
Installation on a supported Debian release is as follows.

1. Install the PacketWarriors software repository if not already
installed for Firefly Logger (i.e. running wsjt2ffdl on a different
system):
```
wget -O/packetwarriors-repo.deb https://repo.packetwarriors.com/packetwarriors-repo.deb
dpkg -i /tmp/packetwarriors-repo.deb
apt update
```

2. Install with apt:
```
apt install wsjt2ffdl
```

3. Upon installation, wsjt2ffdl will be started and enabled to start
on boot. Standard setups should require no additional configuration
beyond the configuration of WSJT-X.

## Configuration
For non-standard configurations, the file `/etc/default/wsjt2ffdl`
may be customized as described within the file. Generally,
this means changing the multicast group IP or the multicast
port.

## Logging
Logs go to systemctl journal and/or syslog (if syslog is installed). 
Logging can be reviewed with the command `journalctl -u wsjt2ffdl`.
Note that the logging will also note any QSOs that are not
able to be stored with FFDL (usually due to dups). There is no way
to feed duplicate QSO alerts back to the user of WSJT-X. Duplicate
QSOs will be ignored by FFDL.

## Troubleshooting
There isn't a lot to troubleshoot aside from configuration.
Starting `wsjt2ffdl` with the `--debug` option can be illuminating
but also noisy. For any issue, open a [GitHub Issue](https://github.com/jxmx/wsjt2ffdl/issues)
and include logging output. Issues without logging output
will be responded to with a request for logging output.