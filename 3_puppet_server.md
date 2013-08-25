!SLIDE
# Puppet Master as nonroot

## 3 Plabs Software
 * Puppet
 * Hiera
 * Facter

!SLIDE
# Puppet Master as nonroot

## Other Software
  * Apache
  * Passenger
  * Libyaml
  * Libapr 

!SLIDE

#Two generations
## First Generation
  * Installed everything to /opt
  * Apache + libapr separate
  * Ruby, yaml separate
  * Puppet, facter, hiera conjoined

!SLIDE

#Two generations
## Problems with first gen
  * No central log location
  * No way to upgrade
  * Conf files akwardly all over the place
  * Rack dir lived under puppet dir

!SLIDE

#Two generations
## New generation
  * Everything rooted under a ``$HOME/local``
  * BSD Ports style
  * Hiera, puppet, facter running from source
  * 'init' scripts for everything in ``local/etc``
  * Logs all go to ``local/var``

!SLIDE

#Installation points
## Use a bash function to expose the puppet command 


    puppet () {
      . $FAKE_ROOT/bin/.ruby_setup.sh

      $FAKE_ROOT/opt/puppet/bin/puppet $@\
      --confdir=$FAKE_ROOT/etc/puppet

    }

!SLIDE

#Installation points
## Passenger 4 reads your .bashrc, check for tty before getting fancy

    if `tty -s`; then
      if env | grep TMOUT >/dev/null; then 
       exec env -u TMOUT bash
      fi 
    fi


!SLIDE

#Installation points
## Set ``LD_LIBRARY_PATH`` and ``RUBYLIB``  at the last possible second, in the puppet function or in etc/init.d/httpd


!SLIDE

#Installation points
##Build passenger on an equivalent system and rsync it up, its dependencies are many, and installing libcurl and openssl from source is hard.

!SLIDE

#Installation points
## Try to keep your env as similar to a rooted environment as you can.
## Tell lies to tell the truth.







