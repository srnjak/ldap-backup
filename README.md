# LDAP Backup Script

This script creates a backup of all OpenLDAP databases specified in the configuration file(s) and stores them in the specified backup directory. 
It also allows you to set a retention policy and send an email notification when the backup is complete.

## Usage

Syntax:

    ldap-backup [-c CONFIG_DIR][-r RETENTION_POLICY][-m] CONF_NAME
    ldap-backup -h

### Options

| Short option | Long option    | Description                                                                                                                                  | Default Value      |
|--------------|----------------|----------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| `-c`         | `--config-dir` | The path to directory containing configurations.                                                                                             | `/etc/ldap-backup` |
| `-r`         | `--retention`  | Retention policy type. Possible values: daily, weekly, monthly, yearly. If set, the value will be used as a suffix in the subdirectory name. |                    |
| `-m`         | `--send-mail`  | Send an email notification after the backup is complete. If this flag is set, make sure to set the email address in the configuration file.  |                    |
| `-h`         | `--help`       | Print help message and exit.                                                                                                                 |                    |

## Configuration
Each configuration file should have a ".cfg" extension and contain the following variables:

- `BACKUP_DIR` - the directory where the backups will be stored
- `LDAP_SUFFIX_LIST` - an array of ldap prefixes to backup
- `EMAILFROM` - email from which the backup summary will be sent
- `EMAILTO` - email where the backup summary will be sent to

## Installation

### Add `srnjak` apt source

To add the `srnjak` apt source to your system, follow these steps:

1. Update the package index:
    ```
    sudo apt-get update
    ```

2. Install the required packages:
    ```
    sudo apt-get install -y ca-certificates gnupg2 curl
    ```

3. Add the `srnjak` repository to your system's package sources:
    ```
    echo "deb https://ci.srnjak.com/nexus/repository/apt-release release main" | sudo tee /etc/apt/sources.list.d/srnjak.list
    ```

4. Add the repository's GPG key to your system's trusted keys:
    ```
    curl -sSL https://ci.srnjak.com/nexus/repository/public/gpg/public.gpg.key | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/srnjak.gpg
    ```

### Install `ldap-backup`

To install the `ldap-backup` package, follow these steps:

1. Update the package index again:
    ```
    sudo apt-get update
    ```

2. Install the `ldap-backup` package:
    ```
    sudo apt-get install -y ldap-backup
    ```
## Dependencies
The script uses the following dependencies:

- `slapd`
- `gzip`
- `mailutils` (optional)

Note: The script will work without `mailutils`, but won't be able to send mail after the backup process is done.
`gzip` is required for compressing and archiving the backup files.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
