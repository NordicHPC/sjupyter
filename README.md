# sjupyter: start Jupyter server in a Slurm queue, making it easy to connect

This doesn't do anything fancy, but makes the running of a Jupyter
notebook server in a Slurm queue a one-command process, and prints
hints on connecting (since that's usually the hardest part).

This module does:

- if the `jupyter` command is not in `PATH`, load some configurable
  default Lmod module.

- Any unknown options get passed to `srun` which executes the script
  on a node.

- Exectute `jupyter notebook`.  Print a help message which describes
  how to connect.

  - Instructions describe how to use "FoxyProxy Standard", which
    allows you to define URL patterns which are forwarded to a SOCKS
    proxy, which is run by ssh.  By using SOCKS+FoxyProxy, we can
    dynamically forward any port to any host without having to already
    know the host/port (and without changing system default proxy).

  - Prints the ssh command to set up the proxy.



## Installation and configuration

Installation: copy the single script to PATH, there are no
dependencies.  The script should be on a shared filesystem, or
non-shared filesystem and link to a shared filesystem.  This is
because the script re-executes itself on the nodes.

Configuration: place a file with the same name as the script, with a
suffix `.conf` next to the absolute path of the script (after
expanding symlinks).



## Development and maintenance

Originally development by Aalto University, but should be considered
beta software: it has worked in testing, but not taken into wide use.
Major contributions welcome.
