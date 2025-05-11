# Software-Only Installation

!!! note "Using the Firefly Logger Appliance is the recommended installation type for Firefly Logger."

!!! tip "You *do not* need to follow these directions *and* "Appliance Installation" "

Firefly Logger has three supported installation types:

* [Raspberry Pi Image](appliance.md) - This is a complete image of Debian 12 for a Pi including a pre-configured 
and ready-out-of-the-box appliance for any Field Day operation. This method is recommended
for most users of Firefly Logger. See [Appliance Installation](appliance.md).

* Debian Packages - Add Firefly Logger to an existing Debian-based Linux installation. Supported
versions are Debian 12 Bookworm (including Raspberry Pi OS 12).

* Manual Installation - Should work on any "LAMP Stack" operating system. Probably works fine
on the BSDs and even Windows although it is not tested there.

## Debian Installation
Installation is supported on Debian 12 Bookworm through use of apt/deb
and the PacketWarriors software repository. Installation on a
supported Debian release is as follows:

1. Install the PacketWarriors software repository:
```
wget -O/tmp/packetwarriors-repo.deb https://repo.packetwarriors.com/packetwarriors-repo.deb
dpkg -i /tmp/packetwarriors-repo.deb
apt update
```

2. Install the logger with apt:
```
apt install firefly-logger
```

3. That's it! The logger should be available on the system at `/firefly-logger`. For
example if on the local Pi it would be http://localhost/firefly-logger.

4. (Optional) To create a "turnkey" system where Firefly Logger is the only
web application and to force Apache to redirect to it, execute:
```
firefly-logger-takeover
```
Note that this will force redirection to a TLS-enabled site (for cookie support)
and you will be prompted for a certificate warning or a "Potential Security Risk
Area" or something of the like. It is safe to accept the warning.

## Manual Installation
Firefly Logger should work on any reasonable Linux system with relatively
modern packages available:

- PHP v8.1 or greater
- MariaDB v10 or greater (should also would work with MySQL 8 unless these platforms diverge further)
- Webserver that supports PHP via FastCGI

This installation assumes the user is reasonably familiar with Linux
and can follow non-step-by-step directions.

1. Install the appropriate pre-requisites: apache 2.4, PHP 8.1+, PHP-FPM, MariaDB 10+

2. Download the latest release .zip file, uncompress, and `cd` to the distribution.

3. Execute `make install`

4. Enable the appropriate system services similar to:
```
systemctl enable php8.2-fpm
systemctl start php8.2-fpm
systemctl enable apache2
systemctl start apache2
systemctl enable mariadb
systemctl start mariadb
```

5. The script `firefly-logger-loaddb` should work to setup the database
depending on the specifics of the system configuration. If not:

    * Create a database and database user for Firefly then
    load `/var/www/firefly-logger/load.sql` into the database.
    For example:

        ```
        CREATE DATABASE ffdl;
        CREATE USER 'ffdl'@'localhost' IDENTIFIED BY 'ffdl';
        GRANT ALL ON ffdl.* TO 'ffdl'@'localhost';
        FLUSH PRIVILEGES;
        USE ffdl;
        SOURCE /var/www/html/load.sql;
        ```

    * Configure the database appropriately in `/var/www/firefly-logger/api/db.php`

6. The logger should be available on the system at `/firefly-logger`. For
example if on the local Pi it would be http://localhost/firefly-logger.

Entering the root user of MariaDB uses the command `mysql -u root ffdl`. If your
database has a password, include `-p` after the word `root`.

## Notes on Upgrading

### Image
Replace the image with a new release image as outlined above.

### Debian Packages
firefly-logger (and wsjt2ffdl) will update with a standard `apt update`
followed by `apt upgrade`.

### Manual Installation
Repeat the installation directions after first deleting
the existing database. It is not recommended to upgrade the database
in-place and is not supported given that any one log is useful
for only up to 48 hours. For the database:

```sql
USE ffdl;
DROP TABLE QSO;
SOURCE /var/www/html/load.sql;
```

In-place upgrades of existing databases are not supported. Ensure all data
has been saved/exported before upgrading the system.
