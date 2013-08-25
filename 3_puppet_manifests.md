!SLIDE

# Package, File, Service

!SLIDE

# Package
## Two basic methods:
  * Wrap an untar command in a defined type
  * Recursive file resource (Puppet Package Manger)

!SLIDE

# Package
## We use both

!SLIDE

    class uti_httpd::base {

      file { "${home_path}/httpd":
        ensure  => directory,
        owner   => $owner,
        group   => $group,
        source  => 'puppet:///modules/uti_httpd',
        recurse => remote
      }
      ...
    }


!SLIDE

    exec {"create-jdk-install-${install_root}":
      command => "/bin/tar xvzf ${tarball_directory}/${jdk_name}",
      cwd     => $install_root,
      creates => "${install_root}/${jdk_create_dir}",
    } 

!SLIDE


# File
  * File Type works strangely when not running as root
  * $owner, $group problem
  * Implementation around 'write' access. 

!SLIDE

    File {
      owner => $owner,
      group => $group,
    }

!SLIDE

    file { $install_root:
      ensure => directory,
    }

    file { "${install_root}/keystore/":
      ensure  => directory,
      require => File[$install_root]
    }



!SLIDE
# Service
  * Possibly the best handled in a rootless environment
  * Can't use real init system.
  * Can use the binary,start,status,stop parameters to great effect
  * I want to look at the path

!SLIDE

    service { 'icinga':
      ensure     => running,
      provider   => base,
      enable     => true,
      hasstatus  => true,
      hasrestart => true,
      start      => "${home_path}/icinga/init/icinga-init start",
      stop       => "${home_path}/icinga/init/icinga-init stop",
      restart    => "${home_path}/icinga/init/icinga-init restart",
      name       => 'icinga'
    }

!SLIDE

# Rootless Module

!SLIDE

# Rootless Module
## Module to provide types and facts to rootless persons
  * tarfile type
  * jdk type
  * facts for user, group, tempdir
  * new file type for rootless environments

!SLIDE


    $tempname = regsubst($name, '/', '-', 'G')

    file { "/var/tmp/${tempname}":
      ensure  => file,
      content => $content,
    }

    exec { "copy-in-${name}":
      command   => 
    "cat /var/tmp/${tempname} > ${name}",
      subscribe => File["/var/tmp/${tempname}"],
      notify    => $notify,
    }


!SLIDE

# Puppet Module Rootless
## GitHub GoGo!

https://github.com/UTIWorldwide/puppet-module-rootless 

puppet module install utiworldwide/rootless
