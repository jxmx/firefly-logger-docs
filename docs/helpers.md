# Helper Scripts
Firefly Logger has several command-line helper scripts to
aid in managing Firefly Logger. These are run from the
shell of the Pi Appliance or the Linux host.

## firefly-logger-clearlog
The script `firefly-logger-clearlog` will erase the contents 
of the database. This script assumes the database is the
standard-named "ffdl". This can be used post-testing to 
clear the database rather than deleting the QSOs by hand.

## firefly-logger-loaddb
The script `firely-logger-loaddb` is used at installation
time to load the initial database. It may be used to repair
a broken installation by an experienced Linux admin but
is not recommended for general use.

## firefly-logger-takeover
The script `firefly-logger-takeover` is used for the
Pi Appliance image to fully reconfigure the webserver
to serve only Firefly Logger. It can be used on other
Linux installs with basic/default webserver configurations
to "takeover" the webserver for Firefly Logger.