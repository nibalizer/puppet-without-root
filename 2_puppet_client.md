!SLIDE

# Installing the Puppet client
  * Libyaml built from source, separate
  * Ruby built from source, separate
  * Puppet and facter from source, together
  * All installed using a --prefix
  
!SLIDE

# Installing the Puppet client
  * Puppet config in:

/opt/app/tibco/opt/puppet/etc/puppet/conf/puppet.conf

  * Ruby/yaml located in

/opt/app/tibco/opt/{ruby,yaml}

!SLIDE

# Installing the Puppet client
  * Drop the whole thing in via a tarball.
  * Massive sed -i on files.

!SLIDE

# Installing the Puppet client
  * Each client is in an environment
  * Conflate UTi environments and puppet environments
  * Puppet vardir, libdir, ssldir all under opt
  * No control over dns so set server = machinename


!SLIDE

# Running the Puppet Client
  * Source a bash file to set ``RUBYLIB``, ``LD_LIBRARY_PATH``
  * Run Puppet with --config argument to pick up the config file, forks to background
  * @reboot cron to fire it up if the machine bounces
    
!SLIDE

# Multi User

  * Sometimes we want to run a service as the ``fico`` user and a separate service as the ``tibco`` on the same machine

!SLIDE

# Certname Abuse

  * Set certname = user-hostname in puppet.conf: ``fico-devbuild1.go2uti.com``
  * Two node definitions in site.pp now
  * Both users have puppet installed under

/opt/app/$USER/opt


