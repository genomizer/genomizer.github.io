# genomizer-admin

* Easy deployment and backup of the Genomizer server and web UI.
    * Implemented using Docker containers.
    * Like a virtual machine, but runs natively.
    * Therefore faster and more lightweight.
    * Actually, three virtual machines linked together.
    * One for the database, one for the server, one for proxy.
    * `genomizer-admin` hides all that complexity from you.

# Deployment

* Just do this:

    ~~~ {.shell}
    $ ssh genomizer
    $ URL="https://github.com/genomizer/genomizer-docker/archive/master.tar.gz"
    $ wget -o genomizer-docker.tar.gz $URL
    $ tar xzf genomizer-docker.tar.gz
    $ cd genomizer-docker
    $ ./genomizer-docker deploy
    ~~~
* This will download the latest release from GitHub.

* Binaries and documentation are now built automatically by our continuous
  integration system.

    * Check out [`github.com/genomizer/genomizer-downloads`](http://github.com/genomizer/genomizer-downloads).

* If some required software is missing, `genomizer-admin` will ask you to run
  `genomizer-admin install-prereqs`.

    * Installs Docker and other software for you.

# Customisation

* Can change which port to listen on and other settings via flags:

    ~~~ {.shell}
    $ ./genomizer-admin help deploy
    [...]

    optional arguments:
    -h, --help            show this help message and exit
    -p PORT, --port PORT  port to listen on (HTTPS)
    --http-port HTTP_PORT port to listen on (HTTP)
    --server-port SERVER_PORT port to listen on (Server)
    -t TMP, --tmp TMP     path to temp directory
    -d DATA, --data DATA  path to data directory
    -c CERT, --cert CERT  path to the certs directory
    --cpu CPU             CPU limit
    --ram RAM             RAM limit
    ~~~

* Several default configurations supported (`production`, `dev1`, `dev2`, ...).

# Backup

* Simply stop the server and copy the `Data/production-data` directory:

    ~~~ {.shell}
    $ ./genomizer-admin stop
    $ tar czf backup.tar.gz /Data/production-data
    $ ./genomizer-admin start
    ~~~
* There is also a WIP `backup` command that does online backup, but it needs
  more testing.

# Other commands, p. 1

* `genomizer-admin info`

    * Information about how the server is configured.

    ~~~ {.shell}
    $ ./genomizer-admin info
    RAM Limit:   -1 (unlimited)
    CPU Limit:   -1 (unlimited)
    Server Port: 7000
    HTTP Port:   80
    HTTPS Port:  443
    Temp dir:    /Data/production-data
    Data dir:    /Data/tmp
    PGDATA dir:  /Data/tmp/pgdata
    Cert dir:    /home/designer/genomizer-docker/production/cert
    ~~~
* `genomizer-admin [stop|start|restart]`

    * Stop/start/restart the server.

* `genomizer-admin destroy`

    * Undo all changes, start from a clean slate.

# Other commands, p. 2

* `genomizer-admin shell`

    * Attach a shell to a running server instance.

    * Choose which container to attach with `-c`:

    ~~~ {.shell}
    $ ./genomizer-admin shell -c postgres
    [...]
    $ ./genomizer-admin shell -c server
    [...]
    $ ./genomizer-admin shell -c nginx
    [...]
    ~~~
* `genomizer-admin logs`

    * Show logs of a running server instance.

    * Again, container can be selected with `-c`.

    * Useful for debugging.

# Getting help

* Just run `genomizer-admin help`

    ~~~ {.shell}
    $ ./genomizer-admin help
    [...]
    subcommands:
  {install-prereqs,deploy,info,stop,start,restart,backup
  ,destroy,shell,logs,debug,help}
    install-prereqs     Install software required for this script to work
    deploy              Deploy the given server configuration
    info                Print information about deployed server instances
    stop                Stop a running server instance
    start               Start a stopped server instance
    restart             Restart a running server instance
    backup              Backup a running server instance
    destroy             Stop and remove a running server instance
    shell               Attach a shell to a running server instance
    logs                Show logs from a running server instance
    debug               Run a debugging shell (advanced)
    help                Show the help message for the given command

    ~~~
* If you find a bug, [file an issue on GitHub against the genomizer-docker repo](https://github.com/genomizer/genomizer-docker/issues/new).

# Live demonstration (if needed)

* A test system is running on [`genomizer2`](https://130.239.192.78).

* These slides are available online on [genomizer.github.io](http://genomizer.github.io).

* Future work:

    * Online backup.
    * Update the PDF manual (which still talks about Vagrant).
    * [Letsencrypt](https://letsencrypt.org/) integration.
        * Get rid of warnings about unsigned certificates.
