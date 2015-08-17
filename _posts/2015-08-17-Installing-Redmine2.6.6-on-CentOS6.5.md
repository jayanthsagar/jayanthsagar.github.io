---
layout: post
title: Installing Redmine2.6.6 on CentOS6.5
---

<p class="message">
CentOS is one of the most frequently chosen Linux operating systems for Linux based production environments. And Redmine is one of the best (if not THE best) open source issue tracking and project management applications, because of it is developed using Ruby on Rails, it feels like complex to deploy for anyone not familiar with the Ruby on Rails environment. The following steps will make the task simple as I am writing it side by doing all of the steps.
</p>

<p>
1.Login to Centos6.5 machine.
</p>
<p>
2.Update packages.
</p>
```html
yum update
```
<p>
3.Reboot the machine after update.
</p>
```html
reboot
```

#### Install the dependencies packages.
<p>
4.These are the basic software packages for environment settings and utility tools to compile other packages in the next section.
</p>
```html
yum -y install nano zip unzip libyaml-devel zlib-devel curl-devel openssl-devel httpd-devel apr-devel apr-util-devel mysql-devel gcc ruby-devel gcc-c++ make postgresql-devel ImageMagick-devel sqlite-devel perl-LDAP mod_perl perl-Digest-SHA
```
#### Install Apache and MySQL
<p>
5.Apache is a server application for communicating over the HTTP protocol. Apache runs on operating systems such as Unix, Linux, Microsoft Windows, and other operating systems.

Apache play an important role in the development of the internet and the world wide web.

MySQL is the database management free open source most popular on the world, MySQL has high speed, stability and ease of use, portability, operating on multiple operating systems offer a large system is very powerful utility functions.

With the speed and high security, MySQL is well suited for applications that access databases on the internet.

Use the following command to install :
</p>
```html

yum -y install httpd mysql mysql-server
```
<p>
Allow start services when OS boot :
</p>
```html
chkconfig httpd on
chkconfig mysqld on
service httpd start
service mysqld start
```
<p>
Set the password for MySQL
</p>
```html
/usr/bin/mysql_secure_installation
```
<p>
Because we not have a password for the root account so you press Enter to skip.
</p>
```html
Enter current password for root (enter for none):
```
<p>
Select Yes to set the password for the MySQL root account.
</p>
```html
Set root password? [Y/n] y
```
<p>
Enter and confirm your password, remove the anonymous user, select Yes
</p>
```html
Remove anonymous users? [Y/n] y
```
<p>
Allow remote login to MySQL as root account, select No.
</p>
```html
Disallow root login remotely? [Y/n] n
```
<p>
Delete the test database, select Yes
</p>
```html
Remove test database and access to it? [Y/n] y
```
<p>
Reload privilege tables, select Yes
</p>
```html
Reload privilege tables now? [Y/n] y
```
#### Turn off SELinux
<p>
SELinux is a security feature advanced for Linux operating system, when installing the system you need to turn off this feature to get the process done smoothly, after successful you can turn on back if you want.
</p>
```html
nano /etc/selinux/config
```
<p>
Change the file content :
</p>
```html
SELINUX=disabled
```
#### Set up the Hostname
<p>
By default when installing a new OS Centos not set the hostname, so we need to setting with the command :
</p>
```html
nano /etc/hosts
```
<p>
Add your domain name or host name that you set on both the command line, save the file and exit, the server name will be changed when restarting.
</p>
#### Configuring the Firewall
<p>
We do not want to turn off the firewall because it's quite important, so you need to add rules to allow port 80 for HTTP and port 443 for HTTPS.

In the Centos OS, you can configuration firewall by editing files iptables and ip6tables.
</p>
```html
nano /etc/sysconfig/iptables
```
<p>
Press Enter to create a new line after the line of port 22, copy the following two commands and right click on the window to the Paste command.
</p>
```html
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
```
<p>
Press CTRL + O to save the file and press CTRL + X to exit.
</p>
<p>
The same applies for IP6 firewall :
</p>
```html
nano /etc/sysconfig/ip6tables
```
<p>
Add these lines to the file.
</p>
```html
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
```
<p>
After you finish editing both files, run the commands to apply the new rules for firewall.
</p>
```html
/etc/init.d/iptables restart
/etc/init.d/ip6tables restart
```
<p>
Allow turn on the firewall when reboot the operating system.
</p>
```html
chkconfig iptables on
chkconfig ip6tables on
```
<p>
Finally, we need to restart the system to apply the changes to the SELinux and Hostname.
</p>
```html
reboot
```
#### Install PHP and phpMyAdmin
<p>
Because we use MySQL database management system, so we need to install phpMyAdmin program management.

phpMyAdmin is a free open source tool written by PHP language to manage MySQL database via a web browser.

It can create, modify or delete databases, tables, fields or records, perform SQL statements, or managing users and permissions.

The command to install PHP and the packages :
</p>
```html
yum -y install php php-mysql php-gd php-imap php-ldap php-mbstring php-odbc php-pear php-xml php-xmlrpc php-pecl-apc php-soap
```
<p>
Restarting the Apache service :
</p>
```html
service httpd restart
```
<p>
And install phpMyadmin :
</p>
```html
rpm --import http://dag.wieers.com/rpm/packages/RPM-GPG-KEY.dag.txt
yum install http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm
yum -y install phpmyadmin
```
<p>
Editing the virtual host file to allow remote login to the phpMyadmin.
</p>
```html
nano /etc/httpd/conf.d/phpmyadmin.conf
```
<p>
Replace text "Allow from 127.0.0.1" to "Allow from all", save the file and exit.
</p>
<p>
Editing the configuration file for the phpMyadmin
</p>
```html
nano /usr/share/phpmyadmin/config.inc.php
```
<p>
Replace text :
</p>
```html
$cfg['Servers'][$i]['auth_type'] = 'cookie';
```
<p>
To :
</p>
```html
$cfg['Servers'][$i]['auth_type'] = 'http';
```
<p>
Save the file and exit, restarting the Apache service :
</p>
```html
service httpd restart
```
<p>
After successfully installed phpMyadmin, you can check at the address :
</p>
```html
http://your-domain/phpmyadmin
```
<p>
Login with account : root / your_password

With Password has been set at step install MySQL database in the above.
</p>
*Note:* If you install the Redmine system on the PC or in a virtual machine which not on the dedicated server, we need to switch the application phpMyadmin to run on port 8080 because port 80 will be used for Redmine in the next steps.

We need to add a port 8080 to the firewall and change the VirtualHost for phpMyadmin.

```html
nano /etc/sysconfig/iptables
```

<p>
Add the command line :
</p>
```html
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
```
<p>
The same applies for IP6 firewall :
</p>
```html
nano /etc/sysconfig/ip6tables
```
<p>
Add the command line :
</p>
```html
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
```
<p>
Restarting firewall service to allow the new port.
</p>
```html
/etc/init.d/iptables restart
/etc/init.d/ip6tables restart
```
<p>
Editing the VirtualHost file to run phpMyadmin on the port 8080
</p>
```html
nano /etc/httpd/conf/httpd.conf
```
<p>
Change the file content :
</p>
```html
<VirtualHost *:8080>
    DocumentRoot /usr/share/phpmyadmin/
    ServerName your_domain.com
</VirtualHost>
```
<p>
Next, add the command to allows listening on the port 8080 in the file "httpd.conf"
</p>
```html
nano /etc/httpd/conf/httpd.conf
```
<p>
Add the command line :
</p>
```html
Listen 8080
```
<p>
Save the file and exit, restarting the Apache service :
</p>
```html
service httpd restart
```
<p>
Now, phpMyadmin will run on the port 8080 at the address :
</p>
```html
http://your-domain:8080
```

#### Install Ruby
<p>
Ruby is a object-oriented programming language, capable of reflection. Syntax inherited from Ada and Perl with object-oriented features of Smalltalk, and also share some features with Python, Lisp, Dylan and CLU, Ruby is a single phase interpreter.

Ruby provides programming patterns, including functional programming, object-oriented, imperative, reflective, it uses dynamic variable and automatic memory management.

Install Ruby interpreter with version management program RVM.
</p>
```html
vim rvm.sh
```
<p>paste the following code into it</p>
```html
#!/usr/bin/env bash

shopt -s extglob
set -o errtrace
set -o errexit

rvm_install_initialize()
{
  DEFAULT_SOURCES=(github.com/rvm/rvm bitbucket.org/mpapis/rvm)

  BASH_MIN_VERSION="3.2.25"
  if
    [[ -n "${BASH_VERSION:-}" &&
      "$(\printf "%b" "${BASH_VERSION:-}\n${BASH_MIN_VERSION}\n" | LC_ALL=C \sort -t"." -k1,1n -k2,2n -k3,3n | \head -n1)" != "${BASH_MIN_VERSION}"
    ]]
  then
    echo "BASH ${BASH_MIN_VERSION} required (you have $BASH_VERSION)"
    exit 1
  fi

  export HOME PS4
  export rvm_trace_flag rvm_debug_flag rvm_user_install_flag rvm_ignore_rvmrc rvm_prefix rvm_path

  PS4="+ \${BASH_SOURCE##\${rvm_path:-}} : \${FUNCNAME[0]:+\${FUNCNAME[0]}()}  \${LINENO} > "
}

log()  { printf "%b\n" "$*"; }
debug(){ [[ ${rvm_debug_flag:-0} -eq 0 ]] || printf "%b\n" "Running($#): $*"; }
fail() { log "\nERROR: $*\n" ; exit 1 ; }

rvm_install_commands_setup()
{
  \which which >/dev/null 2>&1 || fail "Could not find 'which' command, make sure it's available first before continuing installation."
  if
    [[ -z "${rvm_tar_command:-}" ]] && builtin command -v gtar >/dev/null
  then
    rvm_tar_command=gtar
  elif
    ${rvm_tar_command:-tar} --help 2>&1 | GREP_OPTIONS="" \grep -- --strip-components >/dev/null
  then
    rvm_tar_command="${rvm_tar_command:-tar}"
  else
    case "$(uname)" in
      (OpenBSD)
        log "Trying to install GNU version of tar, might require sudo password"
        if (( UID ))
        then sudo pkg_add -z gtar-1
        else pkg_add -z gtar-1
        fi
        rvm_tar_command=gtar
        ;;
      (Darwin|FreeBSD|DragonFly) # it's not possible to autodetect on OSX, the help/man does not mention all flags
        rvm_tar_command=tar
        ;;
      (SunOS)
        case "$(uname -r)" in
          (5.10)
            log "Trying to install GNU version of tar, might require sudo password"
            if (( UID ))
            then
              if \which sudo >/dev/null 2>&1
              then sudo_10=sudo
              elif \which /opt/csw/bin/sudo >/dev/null 2>&1
              then sudo_10=/opt/csw/bin/sudo
              else fail "sudo is required but not found. You may install sudo from OpenCSW repository (http://opencsw.org/about)"
              fi
              pkginfo -q CSWpkgutil || $sudo_10 pkgadd -a $rvm_path/config/solaris/noask -d http://get.opencsw.org/now CSWpkgutil
              sudo /opt/csw/bin/pkgutil -iy CSWgtar -t http://mirror.opencsw.org/opencsw/unstable
            else
              pkginfo -q CSWpkgutil || pkgadd -a $rvm_path/config/solaris/noask -d http://get.opencsw.org/now CSWpkgutil
              /opt/csw/bin/pkgutil -iy CSWgtar -t http://mirror.opencsw.org/opencsw/unstable
            fi
            rvm_tar_command=/opt/csw/bin/gtar
            ;;
          (*)
            rvm_tar_command=tar
            ;;
        esac
    esac
    builtin command -v ${rvm_tar_command:-gtar} >/dev/null ||
    fail "Could not find GNU compatible version of 'tar' command, make sure it's available first before continuing installation."
  fi
  if
    [[ " ${rvm_tar_options:-} " != *" --no-same-owner "*  ]] &&
    $rvm_tar_command --help 2>&1 | GREP_OPTIONS="" \grep -- --no-same-owner >/dev/null
  then
    rvm_tar_options="${rvm_tar_options:-}${rvm_tar_options:+ }--no-same-owner"
  fi
}

usage()
{
  printf "%b" "

Usage

  rvm-installer [options] [action]

Options

  [[--]version] <version>

    The version or tag to install. Valid values are:

      latest         - The latest tagged version.
      latest-minor   - The latest minor version of the current major version.
      latest-<x>     - The latest minor version of version x.
      latest-<x>.<y> - The latest patch version of version x.y.
      <x>.<y>.<z>    - Major version x, minor version y and patch z.

  [--]branch <branch>

    The name of the branch from which RVM is installed. This option can be used
    with the following formats for <branch>:

      <account>/

        If account is wayneeseguin or mpapis, installs from one of the following:

          https://github.com/rvm/rvm/archive/master.tar.gz
          https://bitbucket.org/mpapis/rvm/get/master.tar.gz

       Otherwise, installs from:

         https://github.com/<account>/rvm/archive/master.tar.gz

      <account>/<branch>

        If account is wayneeseguin or mpapis, installs from one of the following:

          https://github.com/rvm/rvm/archive/<branch>.tar.gz
          https://bitbucket.org/mpapis/rvm/get/<branch>.tar.gz

        Otherwise, installs from:

          https://github.com/<account>/rvm/archive/<branch>.tar.gz

      [/]<branch>

        Installs the branch from one of the following:

          https://github.com/rvm/rvm/archive/<branch>.tar.gz
          https://bitbucket.org/mpapis/rvm/get/<branch>.tar.gz

      [--]source <source>

        Defines the repository from which RVM is retrieved and installed in the format:

          <domain>/<account>/<repo>

        Where:

          <domain>  - Is bitbucket.org, github.com or a github enterprise site serving
                      an RVM repository.
          <account> - Is the user account in which the RVM repository resides.
          <repo>    - Is the name of the RVM repository.

        Note that when using the [--]source option, one should only use the [/]branch format
        with the [--]branch option. Failure to do so will result in undefined behavior.

      --trace

        Provides debug logging for the installation script.
Actions

  master - Installs RVM from the master branch at rvm/rvm on github or mpapis/rvm
           on bitbucket.org.
  stable - Installs RVM from the stable branch a rvm/rvm on github or mpapis/rvm
           on bitbucket.org.
  help   - Displays this output.

"
}

## duplication marker 32fosjfjsznkjneuera48jae
__rvm_curl_output_control()
{
  if
    (( ${rvm_quiet_curl_flag:-0} == 1 ))
  then
    __flags+=( "--silent" "--show-error" )
  elif
    [[ " $*" == *" -s"* || " $*" == *" --silent"* ]]
  then
    # make sure --show-error is used with --silent
    [[ " $*" == *" -S"* || " $*" == *" -sS"* || " $*" == *" --show-error"* ]] ||
    {
      __flags+=( "--show-error" )
    }
  fi
}

## duplication marker 32fosjfjsznkjneuera48jae
# -S is automatically added to -s
__rvm_curl()
(
  __rvm_which curl >/dev/null ||
  {
    rvm_error "RVM requires 'curl'. Install 'curl' first and try again."
    return 200
  }

  typeset -a __flags
  __flags=( --fail --location --max-redirs 10 )

  [[ "$*" == *"--max-time"* ]] ||
  [[ "$*" == *"--connect-timeout"* ]] ||
    __flags+=( --connect-timeout 30 --retry-delay 2 --retry 3 )

  if [[ -n "${rvm_proxy:-}" ]]
  then __flags+=( --proxy "${rvm_proxy:-}" )
  fi

  __rvm_curl_output_control

  unset curl
  __rvm_debug_command \curl "${__flags[@]}" "$@" || return $?
)

rvm_error()  { printf "ERROR: %b\n" "$*"; }
__rvm_which(){   which "$@" || return $?; true; }
__rvm_debug_command()
{
  debug "Running($#): $*"
  "$@" || return $?
  true
}
rvm_is_a_shell_function()
{
  [[ -t 0 && -t 1 ]] || return $?
  return ${rvm_is_not_a_shell_function:-0}
}

# Searches the tags for the highest available version matching a given pattern.
# fetch_version (github.com/rvm/rvm bitbucket.org/mpapis/rvm) 1.10. -> 1.10.3
# fetch_version (github.com/rvm/rvm bitbucket.org/mpapis/rvm) 1.10. -> 1.10.3
# fetch_version (github.com/rvm/rvm bitbucket.org/mpapis/rvm) 1.    -> 1.11.0
# fetch_version (github.com/rvm/rvm bitbucket.org/mpapis/rvm) ""    -> 2.0.1
fetch_version()
{
  typeset _account _domain _pattern _repo _sources _values _version
  _sources=(${!1})
  _pattern=$2
  for _source in "${_sources[@]}"
  do
    IFS='/' read -r _domain _account _repo <<< "${_source}"
    _version="$(
      fetch_versions ${_domain} ${_account} ${_repo} |
      GREP_OPTIONS="" \grep "^${_pattern:-}" | tail -n 1
    )"
    if
      [[ -n ${_version} ]]
    then
      echo "${_version}"
      return 0
    fi
  done
}

# Returns a sorted list of all version tags from a repository
fetch_versions()
{
  typeset _account _domain _repo _url
  _domain=$1
  _account=$2
  _repo=$3
  case ${_domain} in
    (bitbucket.org)
      _url=https://${_domain}/api/1.0/repositories/${_account}/${_repo}/branches-tags
      ;;
    (github.com)
      _url=https://api.${_domain}/repos/${_account}/${_repo}/tags
      ;;

    (*)
      _url=https://${_domain}/api/v3/repos/${_account}/${_repo}/tags
      ;;
  esac
  __rvm_curl -s ${_url} |
    \awk -v RS=',' -v FS='"' '$2=="name"{print $4}' |
    sort -t. -k 1,1n -k 2,2n -k 3,3n -k 4,4n -k 5,5n
}

install_release()
{
  typeset _source _sources _url _version _verify_pgp
  _sources=(${!1})
  _version=$2
  debug "Downloading RVM version ${_version}"
  for _source in "${_sources[@]}"
  do
    case ${_source} in
      (bitbucket.org*)
        _url="https://${_source}/get/${_version}.tar.gz"
        _verify_pgp="https://${_source}/downloads/${_version}.tar.gz.asc"
        ;;
      (*)
        _url="https://${_source}/archive/${_version}.tar.gz"
        _verify_pgp="https://${_source}/releases/download/${_version}/${_version}.tar.gz.asc"
        ;;
    esac
    get_and_unpack "${_url}" "rvm-${_version}.tgz" "$_verify_pgp" && return
  done
  return $?
}

install_head()
{
  typeset _branch _source _sources _url
  _sources=(${!1})
  _branch=$2
  debug "Selected RVM branch ${_branch}"
  for _source in "${_sources[@]}"
  do
    case ${_source} in
      (bitbucket.org*)
        _url=https://${_source}/get/${_branch}.tar.gz
        ;;
      (*)
        _url=https://${_source}/archive/${_branch}.tar.gz
        ;;
    esac
    get_and_unpack "${_url}" "rvm-${_branch//\//_}.tgz" && return
  done
  return $?
}

# duplication marker dfkjdjngdfjngjcszncv
# Drop in cd which _doesn't_ respect cdpath
__rvm_cd()
{
  typeset old_cdpath ret
  ret=0
  old_cdpath="${CDPATH}"
  CDPATH="."
  chpwd_functions="" builtin cd "$@" || ret=$?
  CDPATH="${old_cdpath}"
  return $ret
}

get_package()
{
  typeset _url _file
  _url="$1"
  _file="$2"
  log "Downloading ${_url}"
  __rvm_curl -sS ${_url} -o ${rvm_archives_path}/${_file} ||
  {
    _return=$?
    case $_return in
      # duplication marker lfdgzkngdkjvnfjknkjvcnbjkncvjxbn
      (60)
        log "
Could not download '${_url}', you can read more about it here:
https://rvm.io/support/fixing-broken-ssl-certificates/
To continue in insecure mode run 'echo insecure >> ~/.curlrc'.
"
        ;;
      # duplication marker lfdgzkngdkjvnfjknkjvcnbjkncvjxbn
      (77)
        log "
It looks like you have old certificates, you can read more about it here:
https://rvm.io/support/fixing-broken-ssl-certificates/
"
        ;;
      # duplication marker lfdgzkngdkjvnfjknkjvcnbjkncvjxbn
      (141)
        log "
Curl returned 141 - it is result of a segfault which means it's Curls fault.
Try again and if it crashes more than a couple of times you either need to
reinstall Curl or consult with your distribution manual and contact support.
"
        ;;
      (*)
        log "
Could not download '${_url}'.
  curl returned status '$_return'.
"
        ;;
    esac
    return $_return
  }
}

# duplication marker flnglfdjkngjndkfjhsbdjgfghdsgfklgg
rvm_install_gpg_setup()
{
  export rvm_gpg_command
  {
    rvm_gpg_command="$( \which gpg2 2>/dev/null )" &&
    [[ ${rvm_gpg_command} != "/cygdrive/"* ]]
  } ||
    rvm_gpg_command="$( \which gpg 2>/dev/null )" ||
    rvm_gpg_command=""
  debug "Detected GPG program: '$rvm_gpg_command'"
  [[ -n "$rvm_gpg_command" ]] || return $?
}

# duplication marker rdjgndfnghdfnhgfdhbghdbfhgbfdhbn
verify_package_pgp()
{
  if
    "${rvm_gpg_command}" --verify "$2" "$1"
  then
    log "GPG verified '$1'"
  else
    typeset _ret=$?
    log "\
Warning, RVM 1.26.0 introduces signed releases and \
automated check of signatures when GPG software found.
Assuming you trust Michal Papis import the mpapis public \
key (downloading the signatures).

GPG signature verification failed for '$1' - '$3'!
try downloading the signatures:

    ${SUDO_USER:+sudo }${rvm_gpg_command##*/} --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

or if it fails:

    command curl -sSL https://rvm.io/mpapis.asc | ${SUDO_USER:+sudo }${rvm_gpg_command##*/} --import -

the key can be compared with:

    https://rvm.io/mpapis.asc
    https://keybase.io/mpapis
"
    exit $_ret
  fi
}

verify_pgp()
{
  [[ -n "${1:-}" ]] ||
  {
    debug "No PGP url given, skipping."
    return 0
  }

  get_package "$1" "$2.asc" ||
  {
    debug "PGP url given but does not exist: '$1'"
    return 0
  }

  rvm_install_gpg_setup ||
  {
    log "Found PGP signature at: '$1',
but no GPG software exists to validate it, skipping."
    return 0
  }

  verify_package_pgp "${rvm_archives_path}/$2" "${rvm_archives_path}/$2.asc" "$1"
}

get_and_unpack()
{
  typeset _url _file _patern _return _verify_pgp
  _url="$1"
  _file="$2"
  _verify_pgp="$3"

  get_package "$_url" "$_file" || return $?
  verify_pgp "$_verify_pgp" "$_file" || return $?

  [[ -d "${rvm_src_path}/rvm" ]] || \mkdir -p "${rvm_src_path}/rvm"
  __rvm_cd "${rvm_src_path}/rvm" ||
  {
    _return=$?
    log "Could not change directory '${rvm_src_path}/rvm'."
    return $_return
  }

  rm -rf ${rvm_src_path}/rvm/*
  __rvm_debug_command $rvm_tar_command xzf ${rvm_archives_path}/${_file} ${rvm_tar_options:-} --strip-components 1 ||
  {
    _return=$?
    log "Could not extract RVM sources."
    return $_return
  }
}

rvm_install_default_settings()
{
  # Tracing, if asked for.
  if
    [[ "$*" == *--trace* ]] || (( ${rvm_trace_flag:-0} > 0 ))
  then
    set -o xtrace
    rvm_trace_flag=1
  fi

  # Variable initialization, remove trailing slashes if they exist on HOME
  true \
    ${rvm_trace_flag:=0} ${rvm_debug_flag:=0}\
    ${rvm_ignore_rvmrc:=0} HOME="${HOME%%+(\/)}"

  if
    (( rvm_ignore_rvmrc == 0 ))
  then
    for rvmrc in /etc/rvmrc "$HOME/.rvmrc"
    do
      if
        [[ -s "$rvmrc" ]]
      then
        if
          GREP_OPTIONS="" \grep '^\s*rvm .*$' "$rvmrc" >/dev/null 2>&1
        then
          printf "%b" "
  Error: $rvmrc is for rvm settings only.
  rvm CLI may NOT be called from within $rvmrc.
  Skipping the loading of $rvmrc
  "
          exit 1
        else
          source "$rvmrc"
        fi
      fi
    done
  fi

  if
    [[ -z "${rvm_path:-}" ]]
  then
    if
      (( UID == 0 ))
    then
      rvm_user_install_flag=0
      rvm_prefix="/usr/local"
      rvm_path="${rvm_prefix}/rvm"
    else
      rvm_user_install_flag=1
      rvm_prefix="$HOME"
      rvm_path="${rvm_prefix}/.rvm"
    fi
  fi
  if [[ -z "${rvm_prefix}" ]]
  then rvm_prefix=$( dirname $rvm_path )
  fi

  # duplication marker kkdfkgnjfndgjkndfjkgnkfjdgn
  [[ -n "${rvm_user_install_flag:-}" ]] ||
  case "$rvm_path" in
    (/usr/local/rvm)         rvm_user_install_flag=0 ;;
    ($HOME/*|/${USER// /_}*) rvm_user_install_flag=1 ;;
    (*)                      rvm_user_install_flag=0 ;;
  esac
}

rvm_install_parse_params()
{
  install_rubies=()
  install_gems=()
  flags=( ./scripts/install )
  forwarded_flags=()
  while
    (( $# > 0 ))
  do
    token="$1"
    shift
    case "$token" in

      (--trace)
        set -o xtrace
        rvm_trace_flag=1
        flags=( -x "${flags[@]}" "$token" )
        forwarded_flags+=( "$token" )
        ;;

      (--debug|--quiet-curl)
        flags+=( "$token" )
        forwarded_flags+=( "$token" )
        token=${token#--}
        token=${token//-/_}
        export "rvm_${token}_flag"=1
        printf "%b" "Turning on ${token/_/ } mode.\n"
        ;;

      (--path)
        if [[ -n "${1:-}" ]]
        then
          rvm_path="$1"
          shift
        else
          fail "--path must be followed by a path."
        fi
        ;;

      (--branch|branch) # Install RVM from a given branch
        if [[ -n "${1:-}" ]]
        then
          case "$1" in
            (/*)
              branch=${1#/}
              ;;
            (*/)
              branch=master
              if [[ "${1%/}" -ne wayneeseguin ]] && [[ "${1%/}" -ne mpapis ]]
              then sources=(github.com/${1%/}/rvm)
              fi
              ;;
            (*/*)
              branch=${1#*/}
              if [[ "${1%%/*}" -ne wayneeseguin ]] && [[ "${1%%/*}" -ne mpapis ]]
              then sources=(github.com/${1%%/*}/rvm)
              fi
              ;;
            (*)
              branch="$1"
              ;;
          esac
          shift
        else
          fail "--branch must be followed by a branchname."
        fi
        ;;

      (--source|source)
        if [[ -n "${1:-}" ]]
        then
          if [[ "$1" = */*/* ]]
          then
            sources=($1)
            shift
          else
            fail "--source must be in the format <domain>/<account>/<repo>."
          fi
        else
          fail "--source must be followed by a source."
        fi
        ;;

      (--user-install|--ignore-dotfiles)
        token=${token#--}
        token=${token//-/_}
        export "rvm_${token}_flag"=1
        printf "%b" "Turning on ${token/_/ } mode.\n"
        ;;

      (--auto-dotfiles)
        flags+=( "$token" )
        export "rvm_auto_dotfiles_flag"=1
        printf "%b" "Turning on auto dotfiles mode.\n"
        ;;

      (--auto)
        export "rvm_auto_dotfiles_flag"=1
        printf "%b" "Warning, --auto is deprecated in favor of --auto-dotfiles.\n"
        ;;

      (--verify-downloads)
        if [[ -n "${1:-}" ]]
        then
          export rvm_verify_downloads_flag="$1"
          forwarded_flags+=( "$token" "$1" )
          shift
        else
          fail "--verify-downloads must be followed by level(0|1|2)."
        fi
        ;;

      (--autolibs=*)
        flags+=( "$token" )
        export rvm_autolibs_flag="${token#--autolibs=}"
        forwarded_flags+=( "$token" )
        ;;

      (--without-gems=*|--with-gems=*|--with-default-gems=*)
        flags+=( "$token" )
        value="${token#*=}"
        token="${token%%=*}"
        token="${token#--}"
        token="${token//-/_}"
        export "rvm_${token}"="${value}"
        printf "%b" "Installing RVM ${token/_/ }: ${value}.\n"
        ;;

      (--version|version)
        version="$1"
        shift
        ;;

      (head|master)
        version="head"
        branch="master"
        ;;

      (stable)
        version="latest"
        ;;

      (latest|latest-*|+([[:digit:]]).+([[:digit:]]).+([[:digit:]]))
        version="$token"
        ;;

      (--ruby)
        install_rubies+=( ruby )
        ;;

      (--ruby=*)
        token=${token#--ruby=}
        install_rubies+=( ${token//,/ } )
        ;;

      (--rails)
        install_gems+=( rails )
        ;;

      (--gems=*)
        token=${token#--gems=}
        install_gems+=( ${token//,/ } )
        ;;

      (--add-to-rvm-group)
        export rvm_add_users_to_rvm_group="$1"
        shift
        ;;

      (help|usage)
        usage
        exit 0
        ;;

      (*)
        usage
        exit 1
        ;;

    esac
  done

  if (( ${#install_gems[@]} > 0 && ${#install_rubies[@]} == 0 ))
  then install_rubies=( ruby )
  fi

  true "${version:=head}"
  true "${branch:=master}"

  if [[ -z "${sources[@]}" ]]
  then sources=("${DEFAULT_SOURCES[@]}")
  fi

  rvm_src_path="$rvm_path/src"
  rvm_archives_path="$rvm_path/archives"
  rvm_releases_url="https://rvm.io/releases"
}

rvm_install_validate_rvm_path()
{
  case "$rvm_path" in
    (*[[:space:]]*)
      printf "%b" "
It looks you are one of the happy *space* users(in home dir name),
RVM is not yet fully ready for it, use this trick to fix it:

    sudo mkdir -p /${USER// /_}.rvm
    sudo chown -R \"$USER:\" /${USER// /_}.rvm
    echo \"export rvm_path=/${USER// /_}.rvm\" >> \"$HOME/.rvmrc\"

and start installing again.

"
      exit 2
    ;;
    (/usr/share/ruby-rvm)
      printf "%b" "
It looks you are one of the happy Ubuntu users,
RVM packaged by Ubuntu is old and broken,
follow this link for details how to fix:

  http://stackoverflow.com/a/9056395/497756

"
      [[ "${rvm_uses_broken_ubuntu_path:-no}" == "yes" ]] || exit 3
    ;;
  esac

  if [[ "$rvm_path" != "/"* ]]
  then fail "The rvm install path must be fully qualified. Tried $rvm_path"
  fi
}

rvm_install_select_and_get_version()
{
  typeset _version_release

  for dir in "$rvm_src_path" "$rvm_archives_path"
  do
    [[ -d "$dir" ]] || mkdir -p "$dir"
  done

  _version_release="${version}"
  case "${version}" in
    (head)
      _version_release="${branch}"
      install_head sources[@] ${branch:-master} || exit $?
      ;;

    (latest)
      install_release sources[@] $(fetch_version sources[@]) || exit $?
      ;;

    (latest-minor)
      version="$(\cat "$rvm_path/VERSION")"
      install_release sources[@] $(fetch_version sources[@] ${version%.*}) || exit $?
      ;;

    (latest-*)
      install_release sources[@] $(fetch_version sources[@] ${version#latest-}) || exit $?
      ;;

    (+([[:digit:]]).+([[:digit:]]).+([[:digit:]])) # x.y.z
      install_release sources[@] ${version} || exit $?
      ;;

    (*)
      fail "Something went wrong, unrecognized version '$version'"
      ;;
  esac
  echo "${_version_release}" > "$rvm_path/RELEASE"
}

rvm_install_main()
{
  [[ -f ./scripts/install ]] ||
  {
    log "'./scripts/install' can not be found for installation, something went wrong, it usally means your 'tar' is broken, please report it here: https://github.com/rvm/rvm/issues"
    return 127
  }

  # required flag - path to install
  flags+=( --path "$rvm_path" )
  \command bash "${flags[@]}"
}

rvm_install_ruby_and_gems()
(
  if
    (( ${#install_rubies[@]} > 0 ))
  then
    source ${rvm_scripts_path:-${rvm_path}/scripts}/rvm
    source ${rvm_scripts_path:-${rvm_path}/scripts}/version
    __rvm_version

    for _ruby in ${install_rubies[@]}
    do command rvm "${forwarded_flags[@]}" install ${_ruby} -j 2
    done
    # set the first one as default, skip rest
    for _ruby in ${install_rubies[@]}
    do
      rvm "${forwarded_flags[@]}" alias create default ${_ruby}
      break
    done

    for _gem in ${install_gems[@]}
    do rvm "${forwarded_flags[@]}" all do gem install ${_gem}
    done

    printf "%b" "
  * To start using RVM you need to run \`source $rvm_path/scripts/rvm\`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
"

    if
      [[ "${install_gems[*]}" == *"rails"* ]]
    then
      printf "%b" "
  * To start using rails you need to run \`rails new <project_dir>\`.
"
    fi
  fi
)

rvm_install()
{
  rvm_install_initialize
  rvm_install_commands_setup
  rvm_install_default_settings
  rvm_install_parse_params "$@"
  rvm_install_validate_rvm_path
  rvm_install_select_and_get_version
  rvm_install_main
  rvm_install_ruby_and_gems
}

rvm_install "$@"
```
<p>
run the shell script
</p>
```html
sh rvm.sh
```
<p>
After successful, we will launch RVM
</p>
```html
source /etc/profile.d/rvm.sh
```
<p>
The following command will list the versions of Ruby to install :
</p>
```html
rvm list known
```
<p>
We choose the stable version [ruby-] 1.9.3 [-p545], and execute the following command :
</p>
```html
rvm install 1.9.3
```
<p>
The installation process is pretty long time, but you do not need any intervention, after successful, you check with the following command :
</p>
```html
ruby -v
```
#### Install Rubygems
<p>
Rubygems is a Ruby's packages management program, very popular in applications written by Ruby language and the Ruby On Rails framework.
</p>
```html
yum -y install rubygems
```
#### Install Passenger
<p>
The full name of the Passenger is Phusion Passenger, known as mod_rails or mod_rack, it is a web application intergrate with Apache and it can operate as a standalone web server support for the Ruby On Rails applications.

Execute the following commands :
</p>
```html
gem install passenger
passenger-install-apache2-module
```
<p>
After completed, we copy a notification block in the window to create the configuration file in the next steps (select block notification and press C to copy).
</p>
```html
 LoadModule passenger_module /usr/local/rvm/gems/ruby-1.9.3-p551/gems/passenger-5.0.15/buildout/apache2/mod_passenger.so
   <IfModule mod_passenger.c>
     PassengerRoot /usr/local/rvm/gems/ruby-1.9.3-p551/gems/passenger-5.0.15
     PassengerDefaultRuby /usr/local/rvm/gems/ruby-1.9.3-p551/wrappers/ruby
   </IfModule>
```
<p>
Create a new virtual host file for Passenger :
</p>
```html
nano /etc/httpd/conf.d/passenger.conf
```
<p>
Paste the command blocks into the empty file and save it, then restart the Apache service.
</p>
```html
service httpd restart
```
<p>
Create Database for Redmine

Use MySQLAdmin to create an empty database for Redmine, saved password to fill in the configuration file in the next steps.
</p>
```html
mysql --user=root --password=root_password_mysql
create database redmine_db character set utf8;
create user 'redmine_admin'@'localhost' identified by 'your_new_password';
grant all privileges on redmine_db.* to 'redmine_admin'@'localhost';
quit;
```
#### Install Redmine
<p>
Redmine is a main program of the project management system, we will download and install the program from the website of Redmine.

Download Redmine version 2.6.x to directory "/var/www" on the Centos OS.
</p>
```html
cd /var/www
wget http://www.redmine.org/releases/redmine-2.6.6.tar.gz
```
<p>
Extract the folder and rename directory
</p>
```html
tar xvfz redmine-2.6.6.tar.gz
mv redmine-2.6.6 redmine
rm -rf redmine-2.6.6.tar.gz
```

#### Configuring the Database
<p>
The next, we need to configure the database was created from the above steps.
</p>
```html
cd /var/www/redmine/config
cp database.yml.example database.yml
nano database.yml
```
<p>
Enter name for database, enter username and password of the database. Press CTRL + O to save the file and CTRL + X to exit.
</p>
#### Setting up Rails
<p>
Install the package library support for Rails using the Bundle.
</p>
```html
cd /var/www/redmine
gem install bundler
bundle install
rake generate_secret_token
```
<p>
The next, we create the database table for the Redmine application.
</p>
```html
RAILS_ENV=production rake db:migrate
RAILS_ENV=production rake redmine:load_default_data
```
#### Activate FCGI

```html
cd /var/www/redmine/public
mkdir plugin_assets
cp dispatch.fcgi.example dispatch.fcgi
cp htaccess.fcgi.example .htaccess
```

#### Setting up Apache and FastCGI

```html
cd /var/www/
rpm --import https://fedoraproject.org/static/0608B895.txt
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
rpm -ivh epel-release-6-8.noarch.rpm
yum -y install mod_fcgid
rm -rf epel-release-6-8.noarch.rpm
```

#### Creating Files Directory
<p>
This directory contains data files generated during the operation of Redmine as document or image file, we create a new directory in the "/opt".
</p>

```html
mkdir -p /opt/redmine/files
chown -R apache:apache /opt/redmine
cd /var/www/redmine/config
cp configuration.yml.example configuration.yml
nano configuration.yml
```

<p>
Enter the directory path containing the data files you just created in the previous step into the line "attachments_storage_path".
</p>
*Note:* You must add a space at the begin of the path "/opt/redmine/files" after character ":"
#### Configuring Email
<p>
Another very important function of Redmine is using email to notify members when the contents of each project changes, Redmine can use many different methods to send email that is Sendmail, SMTP, GMail ...

To configure the email we will edit the configuration file.
</p>

```html
nano /var/www/redmine/config/configuration.yml
```

<p>
The simplest is you use features of the default SendMail in the Centos OS by settings :
</p>

```html
  email_delivery:
   delivery_method: :sendmail
```

*Note :* Do not use the Tab key to indent when editing the configuration file, you need to use the space bar on the keyboard.
<p>
If you use GMail's SMTP, you need to register an email account with the login methods used password normal and disable two-step authentication by smart phone.
</p>
<p>
Enter your Gmail account as below :
</p>

```html
  email_delivery:
   delivery_method: :smtp
   smtp_settings:
        enable_starttls_auto: true
        address: "smtp.gmail.com" 
        port: 587
        domain: "smtp.gmail.com" 
        authentication: :plain
        user_name: "your_email@gmail.com" 
        password: "your_password" 
```

<p>Save and exit</p>
#### Configuring SMTP
<p>
    Configure sendmail on the machine so that it can send emails. In this particular case sendmail is configured to use mail.foo.com as SMART_HOST for sending emails outside.
</p>

```html

cp {REDMINE_ROOT}/config/configuration.yml.example {REDMINE_ROOT}/config/configuration.yml

```

<p>
    Edit the config/configuration.yml file and near the end where production: is present use:
</p>

```html
            production:
               email_delivery:
                 delivery_method: :sendmail
```

-    Login as administrator in redmine and go to "Administration -> Settings -> Email notifications" and choose appropriate defaults. In current configuration settings are:
-        Emission email address:redmine@issues.foo.com
-        Blind carbon copy for receipients
-        Default notification: Only for things I am involved in
-        Email notifications should be sent for issue added or issue updated (note, status, priority) 
<p> </p>
#### LDAP authentication

- Login as administrator
- Navigate to Administration > LDAP authentication
- Click on "New authentication mode"
- Enter the below values in corresponding fields

```html
        Host: ldap.virtual-labs.ac.in 
        Port: 389 
        Base DN: dc=virtual-labs,dc=ac,dc=in 
        Login: uid 
        "On-the-fly user creation" checkbox 
```

-    Save and Test the connection
-    Login with an LDAP account
-    Provide the additional details when prompted (name, email, etc.)
-    Logout and login as administrator
-    Navigate to Administration > Users
-    Click on the user name used in last step
-    Go to the Projects tab
-    Choose a project name and select a role
-   Click on Add to give the LDAP user access to the redmine project 

#### Create Virtual Host for Redmine

Create an Apache configuration file for the Redmine application at the port 80.

nano /etc/httpd/conf.d/redmine.conf

Copy the text below and paste into the editor window, note the information to change your domain name.

```html

<VirtualHost *:80>
        ServerName your_domain
        ServerAdmin your_domain@domain.com
        DocumentRoot /var/www/redmine/public/
        ErrorLog logs/redmine_error_log
        <Directory "/var/www/redmine/public/">
                Options Indexes ExecCGI FollowSymLinks
                Order allow,deny
                Allow from all
                AllowOverride all
        </Directory>
</VirtualHost>

```

Save the file configuration and exit.
#### Running Redmine

Before execute Redmine in the first time, we must permission for the directory installed Redmine and restart Apache service.

```html

cd /var/www
chown -R apache:apache redmine
chmod -R 755 redmine
service httpd restart
```
Redmine will run at the following address URL :

```html

http://your-domain

```

Login to system with an administrator account : admin / admin

You can change your password after successful login.

We can see Redmine has running but very primitive, in the next steps we will install the support plugins and customized Redmine to use professional.
