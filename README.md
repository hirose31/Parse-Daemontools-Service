<a href="https://travis-ci.org/hirose31/Parse-Daemontools-Service"><img src="https://travis-ci.org/hirose31/Parse-Daemontools-Service.png?branch=master" alt="Build Status" /></a>
<a href="https://coveralls.io/r/hirose31/Parse-Daemontools-Service?branch=master"><img src="https://coveralls.io/repos/hirose31/Parse-Daemontools-Service/badge.png?branch=master" alt="Coverage Status" /></a>

# NAME

Parse::Daemontools::Service - Retrieve status and env of service under daemontools

# INSTALLATION

To install this module, run the following commands:

    perl Build.PL
    ./Build
    ./Build test
    ./Build install

# SYNOPSIS

Normally, Parse::Daemontools::Service requires root privileges because need to read /service/DAEMON/supervise/status file.

    use Parse::Daemontools::Service;
    
    my $ds = Parse::Daemontools::Service->new;
    my $status = $ds->status("qmail");
    
    my $status = $ds->status("my-daemon",
                             {
                                 env_dir => "/service/my-daemon/my-env-dir",
                             });
    
    my $status = $ds->status("my-daemon",
                             {
                                 env_dir => [
                                     "/service/my-daemon/env",
                                     "/service/my-daemon/my-env-dir",
                                 ],
                             });

# DESCRIPTION

Parse::Daemontools::Service retrieves status and env of service under daemontools.

# METHODS

- __new__({ base\_dir => Str })

    base\_dir (optional): base directory of daemontools. Default is "/service"

- __status__($service\_name:Str, { env\_dir => Str|ArrayRef\[Str\] })

    Return status and env of $service\_name as following HashRef.

        +{
            service  => Str, # "/service/my-daemon"
            pid      => Int, # PID of daemon process
            seconds  => Int, # uptime of this daemon process
            start_at => Int, # UNIX time of this daemon process started at
            status   => Str, # "up" | "down"
            info     => Str, # "" | "normally down" | "normally up" | ...
            env      => {    # environment variables in envdir
                BAR => "bar",
                FOO => "foo"
            },
        },

    Default env\_dir is "/service/my-daemon/env". You can specify env\_dir(s) by Str or ArrayRef. When you specify more than one env\_dirs, same key are overridden by latter env\_dir.

# AUTHOR

HIROSE Masaaki <hirose31@gmail.com>

# REPOSITORY

[https://github.com/hirose31/Parse-Daemontools-Service](https://github.com/hirose31/Parse-Daemontools-Service)

    git clone git://github.com/hirose31/Parse-Daemontools-Service.git

patches and collaborators are welcome.

# SEE ALSO

svstat(1)

# COPYRIGHT

Copyright HIROSE Masaaki

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.
