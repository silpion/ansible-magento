# ansible magento
can install/configure magento in different versions ( optional install  patches ).  
Additional can install modman, install Extensions from git and link given modman Extensions.

## Variables

### magento_root: /var/www/magento
set magento-root dir 

### magento_download: "yes"
if you have for example special magento-version local you can put it into files directory and 
configure magento_download: "no"

### magento_install_tmp_dir: /tmp 
tmp - directory on remote for installation stuff

### magento_version: "1.7.0.2"
configure magento-version for installation

### magento_webuser: "www-data" 
### magento_webgroup: "www-data" 
set magento webuser and group

### magento_configs: []
setup magento install-configuration

Defaults:
```
magento_configs: []
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
   skip_url_validation: "no"   // set this to "yes" if you install nginx as webserver

```

Example ( change db access and disable secure ):
```
magento_configs: []
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
### magento_patch_files: []
list for magento patches

Example (will install magento-patch for php5.4  magento(1.7.0.2)):
```
magento_patch_files: 
  - url: "http://www.magentocommerce.com/downloads/assets/ce_patches/PATCH_SUPEE-2629_EE_1.12.0.0_v1.sh"
```
### magento_modman: "yes"
if is set to yes it will install modman and install given extensions (only from git)

### magento_modman_bin_dir: "/root/bin"
where modman should be installed ?

### magento_modman_extensions_git: []
list of extensions witch will be cloned from git-repository and linked into magento via modman

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
  - repo: "https://github.com/path/to/repo.git"
    dest: "/var/www/extensions/repo"
  - repo: "https://github.com/path/to/repo2.git
    dest: "/var/www/extensions/repo2"
    remote: "test"
```

### magento_modman_extensions_other: []
list of non-git extensions for modman to link

Required Variables in list: 
```
dest
```

Example:
```
magento_modman_extensions_other:
  - dest: "/var/www/extensions/extension"
  - dest: "/var/www/extensions/extension2"
```

## Dependencies
see magento system requirements: http://magento.com/resources/system-requirements

## License
Apache Version 2.0

## Author Information
Anja Siek @anja.siek silpion.de
