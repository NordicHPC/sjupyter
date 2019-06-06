# sjupyter: start Jupyter server in a Slurm queue, making it easy to connect

This doesn't do anything fancy, but makes the running of a Jupyter
notebook server in a Slurm queue a one-command process, and prints
hints on connecting (since that's usually the hardest part).  [Example
user instructions](https://scicomp.aalto.fi/triton/apps/sjupyter.html)
which could be moved to here.

If you have a HPC JupyterHub, this module has very little use (only
allowing users to run their own Jupyter in other partitions beyond
what you support through the hub).

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

To clarify, this is the message it prints after typing `srun` with no
arguments (it could be improved, may be different in the current
code):

```
********************
* Starting jupyter.  To cancel, C-c C-c twice within one second (to kill
* srun).
*
* You now need to connect to the jupyter  The direct way: ssh to
* %(CLUSTER_NAME)s with
*   ssh -L %(port)d:%(hostname)s:%(port)d %(CLUSTER_SSH)s
* and use the URL below, but change %(hostname)s to localhost
*
* Even better, use SSH proxy:
*   'ssh -D 8123 %(CLUSTER_SSH)s'
* For Firefox, install the extension FoxyProxy Standard,
* Set up a new proxy rule for the pattern %(CLUSTER_INTERNAL_PATTERN)s, using
*   localhost:8123 type SOCKS5,
* Now any %(CLUSTER_NAME)s jupyter URLs automatically go to your notebook!
* OR:
* Create a new browser instance,
* Change the proxy of that browser to localhost:8123, SOCKS5,
* Connect to jupyter using the exact URL that is printed below.
* %(CLUSTER_NETWORK_CONNECT)s
********************
```



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
