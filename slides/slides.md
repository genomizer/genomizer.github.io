# genomizer-admin

* Easy deployment and backup of the Genomizer server and web UI.
    * Implemented using Docker.
    * Like a virtual machine, but more light-weight.
    * Hides the complexities of Docker.

# Deployment

* Just do this:

    ~~~ {.shell}
    $ ssh my_server
    $ URL="https://github.com/genomizer/genomizer-docker/archive/master.tar.gz"
    $ wget -o genomizer-docker.tar.gz $URL
    $ tar xf genomizer-docker.tar.gz
    $ cd genomizer-docker
    $ ./genomizer-docker deploy
    ~~~
* This will download the latest release from GitHub.

* If some required software is missing, it will ask you to run `genomizer-admin
  install-prereqs`.

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

# Backup

* Also easy:

    ~~~ {.shell}
    $ ./genomizer-admin backup -o backup_dir
    ~~~

* To restore from backup:

    ~~~ {.shell}
    $ ./genomizer-admin deploy -r backup_dir
    ~~~
* Right now only backs up the database.

* To find out where data files are located, run `genomizer-admin info`.

# Other commands, p. 1

* `genomizer-admin info`

    ~~~ {.shell}
    $ ./genomizer-admin info
    TODO
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

# Documentation

* Just run `genomizer-admin help`

    ~~~ {.shell}
    $ ./genomizer-admin help
    subcommands:
  {install-prereqs,deploy,info,stop,start,restart,backup,destroy,shell,logs,debug,help}
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

# Live demonstration (if needed)

* A test system is running on [`https://130.239.192.78`](https://130.239.192.78)
