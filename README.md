# ansible magento

Install/configure magento in different versions (including optional installation of patches).
Allows to configure extensions to be managed with modman.

## Variables

* ``magento_version``: magento version to install (string, default: ``1.7.0.2``)
* ``magento_root``: Configure magento root directory (string, default: ``/var/www/magento``)
* ``magento_user``: Service user to run magento with (string, default: ``www-data``)
* ``magento_group``: Service group for the ``magento_user`` service user (string, default: ``www-data``)
* ``magento_download``: Whether to download magento (boolean, default: ``true``)
* ``magento_download_host``: Mirror URI where to download magento from when ``magento_download`` is true (string, default: ``http://www.magentocommerce.com/downloads/assets/{{ magento_version }}``)
* ``magento_download_package``: Package name for the magento redistributable archive (string, default: ``magento-{{ magento_version }}.tar.gz``)
* ``magento_download_tmp_dir``: Where to download magento redistributable archive to (string, default: ``/tmp``)
* ``magento_download_tmp_dir_mode``: Filesystem access mode for the download directory (oct, default: ``2777``)
* ``magento_copy``: Whether to recursively copy an existing and extracted magento tree from the workstations filesystem (boolean, default: ``false``)
* ``magento_copy_local_dir``: Source directory for an existing and extracted magento tree to copy to the managed node (string, default: ``""``)
* ``magento_install``: Whether to install magento even if app/etc/local.xml does not exist (boolean, default: ``false``)
* ``magento_install_tmp_dir``: Destination directory on the managed node to upload magento data to (string, default: ``/tmp``)
* ``magento_install_tmp_dir_sub``: Allows to configure a known base directory name within the redistributable archive (string, default: ``magento``)
* ``magento_permissions_list``: Allows to configure access controls for magento internal directory structure (dict, default: ``[]``)
* ``magento_config``: Configure magento (dict, default: see below)
* ``magento_patch_files``: Configure list of patches to apply into magento (list, default: ``[]``)
* ``magento_modman``: Wether to install modman into magento (boolean, default: ``true``)
* ``magento_modman_bin_dir``: Directory where to install modman to (string, default: ``/root/bin``)
* ``magento_modman_extensions_git``: List of extensions to be installed/managed with modman from Git resources (dict, default: ``[]``)
* ``magento_modman_extensions_other`` List of non-git extensions to be managed with modman (dict, default: ``[]``)


### magento_copy

This variable controls two different meanings.

#### true

``true`` here means that there is a local filesystem on the workstation where an extracted magento is hosted. This allows to pre-configure
a magento redistributable and have this role install this version of magento recursively. Enabling this feature requires to configure
``magento_copy_local_dir`` variable and point it to the extracted magento installation.

#### false

If this variable is set false (default) this role will copy a magento redistributable archive to the remote node. Archive source is
``{{ magento_download_tmp_dir }}/{{ magento_download_package }}``.
When configuring ``magento_dowload`` to ``false`` this allows to install a pre-configured magento tarball to the managed node.


Running the default role configuration will download magento for magentocommerce website, install the archive to the managed node
and extract it to ``{{ magento_root }}`` directory.

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
    dest: /var/www/extensions/repo
  - repo: https://github.com/path/to/repo2.git
    dest: /var/www/extensions/repo2
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

See magento system requirements: http://magento.com/resources/system-requirements

## License

Apache Version 2.0

## Author Information

Anja Siek @anja.siek silpion.de


<!-- vim: set nofen ts=4 sw=4 et: -->
