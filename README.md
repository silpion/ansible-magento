# ansible magento

Install/configure magento in different versions (including optional installation of patches).
Allows to configure extensions to be managed with modman.

## Variables

* ``magento_version``: magento version to install (string, default: ``1.7.0.2``)
* ``magento_root``: Configure magento root directory (string, default: ``/var/www/magento``)
* ``magento_bin_dir``: Directory to save binary-files like istall-script (string, default: ``/root/bin``)
* ``magento_webuser``: Service user to run magento with (string, default: ``www-data``)
* ``magento_webgroup``: Service group for the ``magento_webuser`` service user (string, default: ``www-data``)
* ``magento_download``: Whether to download magento (boolean, default: ``true``)
* ``magento_download_host``: Mirror URI where to download magento from when ``magento_download`` is true (string, default: ``http://www.magentocommerce.com/downloads/assets/{{ magento_version }}``)
* ``magento_download_package``: Package name for the magento redistributable archive (string, default: ``magento-{{ magento_version }}.tar.gz``)
* ``magento_download_url``: Full Download-Url (string, default: ``{{ magento_download_host }}/{{ magento_download_package }}``)
* ``magento_install``: Whether to install magento even if app/etc/local.xml does not exist (boolean, default: ``false``)
* ``magento_archive_subdir``: Allows to configure a known base directory name within the redistributable archive (string, default: ``magento``)
* ``magento_permissions_list``: Allows to configure access controls for magento internal directory structure (dict, default: ``[]``)
* ``magento_config``: Configure magento (dict, default: see below)
* ``magento_patch_files``: Configure list of patches to apply into magento (list, default: ``[]``)
* ``magento_manage_magento``: If you only want to manage modman extensions set this to false and all the magento-suff will not run (boolean, default: ``true``)
* ``magento_modman``: Wether to install modman into magento (boolean, default: ``true``)
* ``magento_modman_bin_dir``: Directory where to install modman to (string, default: ``magento_bin_dir``)
* ``magento_modman_extensions_git``: List of extensions to be installed/managed with modman from Git resources (dict, default: ``[]``)
* ``magento_modman_extensions_other`` List of non-git extensions to be managed with modman (dict, default: ``[]``)


### magento_permissions_list

Allows to configure access crontrols for magento internal directory structure.

```
magento_permissions_list:
  - mode: "2771"
    type: "d"
  - mode: "0644"
    type: "f"
```

### magento_config

Configure magento installation with the following defaults:

```
magento_config: []
   license_agreement_accepted: "yes"
   locale: "de_DE"
   timezone: "Europe/Berlin"
   default_currency: "EUR"
   db_host: "localhost"
   db_name: "magento"
   db_user: "magento"
   db_pass: "magento"
   url: "http://localhost:8080/"
   use_rewrites: "yes"
   use_secure: "yes"
   secure_base_url: "https://localhost:8080"
   use_secure_admin: "yes"
   admin_firstname: "Magento"
   admin_lastname: "Admin"
   admin_email: "magento@test.de"
   admin_username: "admin"
   admin_password: "magento12"
   skip_url_validation: "no"

```

**NOTE**: Configure ``magento_config.skip_url_validation`` to ``true`` if you install nginx as web server.


Example (configure database and disable ssl for admin panel):

```
magento_config: []
   db_host: "localhost"
   db_name: "test"
   db_user: "testuser"
   db_pass: "password"
   url: "http://magento-local.de/"
   use_secure: "no"
   secure_base_url: ""
   use_secure_admin: "no"
   skip_url_validation: "yes"
```

### magento_patch_files

Configure list of patches to apply into magento, e.g. install magento-patch for php5.4 in magento version 1.7.0.2.

```
magento_patch_files:
  - url: "http://www.magentocommerce.com/downloads/assets/ce_patches/PATCH_SUPEE-2629_EE_1.12.0.0_v1.sh"
```

### magento_modman_extensions_git

List of extensions witch will be cloned from git and linked into magento with modman.

Required Variables in list:

```
repo
name
dest
```

Optional variables (defaults)

```
version: "HEAD"
remote: "origin"
key_file: None
ssh_opts: None
accept_hostkey: "no"
bare: "no"
depth: 0
executable: ""
reference: ""
gitupdate: "yes"
force: "yes"

```

Example:

```
magento_modman_extensions_git:
  - repo: https://github.com/path/to/repo.git
    name: "repo"
    dest: /var/www/extensions
  - repo: https://github.com/path/to/repo2.git
    name: "repo2"
    dest: /var/www/extensions
    remote: test
    force: no
```

### magento_modman_extensions_other

List of non-git extensions for modman to link.

Required Variables in list:

```
dest
```

Example:

```
magento_modman_extensions_other:
  - dest: /var/www/extensions/extension
  - dest: /var/www/extensions/extension2
```

## Dependencies

### soft Dependencies 

See magento system requirements: http://magento.com/resources/system-requirements

### hard Dependencies

This role depends on [silpion.lib](https://github.com/silpion/ansible-lib)
role. This is configured for ``ansible-galaxy install`` in **requirements.yml**.

**NOTE**: Check lib configuration Variables to configure privilege escalation, download/upload -path etc.

**NOTE**: Requirements are installed as virtual user ``silpion``
(``silpion.lib``).

Be sure to install required roles with

    ansible-galaxy install --role-file requirements.yml

## License

Apache Version 2.0

## Author Information

Anja Siek @anja.siek silpion.de

<!-- vim: set nofen ts=4 sw=4 et: -->
