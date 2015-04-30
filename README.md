# ansible magento
can install/configure magento in different versions ( optional install  patches ).  
Additional can install modman, install Extensions from git and link given modman Extensions.

## Variables

### magento_root: /var/www/magento
set magento-root dir 

### magento_download: "yes"
select yes for downloading from remote. If is set to no you need to cp tar.gz to magento_download_tmp_dir

### magento_download_host: "http://www.magentocommerce.com/downloads/assets/{{ magento_version }}"
configure host. Default is set to official download location, for configured version

### magento_download_package: "magento-{{ magento_version }}.tar.gz"
set download packagename

### magento_copy: "no"
if you have for example special magento-version local you can put it into directory specified 
in  magento_copy_local_dir: and configure magento_copy: "yes" and magento_download: "no"

### magento_copy_local_dir: ""
difine local magento source

### magento_download_tmp_dir: "/tmp"
specify where magento should be downloaded local

### magento_download_tmp_dir_mode: "2777"
configure mode of download tmp dir

### magento_install_tmp_dir: "/tmp" 
tmp - directory on remote for installation stuff

### magento_install_tmp_dir_sub: "magento"
configure subdirectory name in tar.gz 

### magento_version: "1.7.0.2"
configure magento-version for installation

### magento_webuser: "www-data" 
### magento_webgroup: "www-data" 
set magento webuser and group

### magento_permissions_list: []
set permissions for example for directorys and files

Example:

```
magento_permissions_list:
  - mode: "2771"
    type: "d"
  - mode: "0644"
    type: "f"
``` 

### magento_config: []
setup magento install-configuration

Defaults:

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
   skip_url_validation: "no"   // set this to "yes" if you install nginx as webserver

```

Example ( change db access and disable secure ):

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
### magento_install: false
role will only run install magento if magento_install is set true 
or if app/etc/local.xml not exists

## Dependencies
see magento system requirements: http://magento.com/resources/system-requirements

## License
Apache Version 2.0

## Author Information
Anja Siek @anja.siek silpion.de
