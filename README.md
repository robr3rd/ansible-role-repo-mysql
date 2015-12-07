# Ansible Role: MySQL Yum Repository
Installs the [MySQL Yum Repository](https://dev.mysql.com/downloads/repo/yum/) and sets the appropriate MySQL subrepository version as the active one. This enables the installation of newer MySQL versions otherwise unavailable to the OS. For instance, as of this writing all MySQL versions above 5.1.73 (release date: 2013-12-03) are currently unavailable on CentOS 6.7 (release date: 2015-08-07) out of the box.

Once installed, any packages in the repository whose names conflict with those available by default will take precedence over the defaults.

**Note:** Currently only supports "Enterprise Linux" (e.g. RedHat, CentOS, etc.) although pull requests for greater compatibility are, of course, welcome.

## Requirements
None

## Role Variables
### General
#### Desired MySQL Version
```yaml
mysql_target_version: "5.6"
```

### Yum Package Manager
#### MySQL Repository Family
```yaml
mysql_repo_family: "el" # Enterprise Linux (RedHat `ansible_os_family`)
# or
mysql_repo_family: "fc" # Fedora
```

#### MySQL Yum Repository Version
The most recent release of MySQL supported by the repository (not necessarily MySQL -- although the version numbers will often align). This is because only the most recent one is maintained (which is fine for compatibility since the older ones are bundled into the package and are simply disabled by default).

```yaml
mysql_repo_version: "5.7"
```

#### MySQL Yum Repository Location
The MySQL Yum Repository install source (by default, the URL to the MySQL Yum Repository).

In the case of this role's default values, the URL is dynamically built based on a templated official repository link and uses `ansible_distribution_version`, `mysql_repo_family`, and `mysql_repo_version` to fill in some blanks. To verify or otherwise obtain this link yourself, navigate to [the official MySQL Yum Repository page](http://dev.mysql.com/downloads/repo/yum/) and click "Download" for the desired OS/MySQL combination. On the following page, the repository is linked to by the, "No thanks, just start my download." link. To get the path from that link, simply right-click it and choose "Copy Link Address".

This could also be a local path if you would like to supply your own repository.

```yaml
mysql_repo_location: "http://dev.mysql.com/get/mysql{{ mysql_repo_version | replace(".", "") }}-community-release-{{ mysql_repo_family }}{{ ansible_distribution_version | replace(".", "-") }}.noarch.rpm"
```

##### Troubleshooting
If problems are encountered with the path not being found, verify that `mysql_repo_version` is, in fact, the latest version since the MySQL Yum Repository team only maintains the most recent version.

#### Local MySQL Yum Repository Install Location
Where the MySQL Yum Repository gets saved on the machine. This should only need to change if you have explcitly customized the installation location.

```yaml
mysql_repo_local_install: "/etc/yum.repos.d/mysql-community.repo"
```

## Usage
After making this role available to your playbook, it may be called with something like:

```yaml
- hosts: servers
- roles:
	- { role: robr3rd.repo-mysql }
```

## License
The MIT License (MIT)
