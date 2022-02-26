# perl 


## Installing

rhel based. 
```
yum install -y perl
```

debian based.
```
apt install -y perl
```

### There are also perl modules in the distro package manager.

rhel based installing sftp client
```
yum install -y perl-Net-SFTP-Foreign
```
debian based installing sftp client
```
apt install -y libnet-sftp-foreign-perl
```

### Setup CPAN for installing perl modules

rehl based
```
 dnf install -y  perl-CPAN
```

debian based.
```
apt install -y libpath-tiny-perl
```

Then type cpan in your terminal to setup what it needs just press enter.

```
| => cpan


Loading internal logger. Log::Log4perl recommended for better logging

CPAN.pm requires configuration, but most of it can be done automatically.
If you answer 'no' below, you will enter an interactive dialog for each
configuration option instead.

Would you like to configure as much as possible automatically? [yes] 

```
THen you may see this just press enter or if you are a advanced user do your thing.

```
Use of uninitialized value $what in concatenation (.) or string at /usr/share/perl/5.30/App/Cpan.pm line 679, <STDIN> line 1.
 <install_help>

Warning: You do not have write permission for Perl library directories.

To install modules, you need to configure a local Perl library directory or
escalate your privileges.  CPAN can help you by bootstrapping the local::lib
module or by configuring itself to use 'sudo' (if available).  You may also
resolve this problem manually if you need to customize your setup.

What approach do you want?  (Choose 'local::lib', 'sudo' or 'manual')
 [local::lib] 

```


### Then to install perl module from cpan.


In this example is the proxmox module.

```
perl -MCPAN -e 'install Net::Proxmox::VE'
```

### Running Perl programs

To run a Perl program from the Unix command line:

```
perl progname.pl
```

Alternatively, put this as the first line of your script:
```
#!/usr/bin/env perl
```

... and run the script as /path/to/script.pl. Of course, it'll need to be executable first, so chmod 755 script.pl (under Unix).

(This start line assumes you have the env program. You can also put directly the path to your perl executable, like in #!/usr/bin/perl).

For more information, including instructions for other platforms such as Windows and Mac OS, read perlrun.
Safety net

Perl by default is very forgiving. In order to make it more robust it is recommended to start every program with the following lines:
```
#!/usr/bin/perl
use strict;
use warnings;
```


### Links

https://perldoc.perl.org/perlintro
