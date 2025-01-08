# Annex

## Recommendation: 'watch' command and Windows

In several labs you will be prompted to run a command (`kubectl` or other) preceded by the unix command `watch`. `watch` is a command allowing to execute a program periodically by displaying the result on the screen which allows to see the evolution of the results of this command over time. There is no `watch` equivalent on Windows, however several options are available for Windows users:

- Use a local session [MobaXterm](https://mobaxterm.mobatek.net/), which launches a shell [Cygwin](https://www.cygwin.com/) where the `watch` command is available because preinstalled. Note: [MobaXterm](https://mobaxterm.mobatek.net/) is also part of the recommended software to launch an ssh connection, see next section.
- Install the `watch` package under [Cygwin](https://www.cygwin.com/) if the latter (or equivalent) is already available on your computer
- Go through the _PowerShell_ `Watch` command provided by the module whose source code is available in the existing `watch-for-windows-ps` directory in the zip provided to start the Labs. To be able to use this module, place the file `Watch.psm1` in the directory `%USERPROFILE%\Documents\WindowsPowerShell\Modules\Watch`. You will then need to run all `kubectl` commands from a _PowerShell_ console. This `Watch` module does not however work quite like the `watch` Linux command, you will have to add an additional parameter to specify the refresh rate (which is 2 seconds by default with `watch` under Linux): `watch 2 kubectl get pods`
- If none of the previous options is possible, you can replace the `watch kubectl get` commands with `kubectl get <xx> -w` which allows you to wait for modification in the `kubectl` command but with a less readable output than the `watch` command

## Recommendation: on Windows, use a MobaXterm/PuTTY type terminal for ssh commands

- If you are on Windows, we strongly advise you to go through a terminal like [MobaXterm](https://mobaxterm.mobatek.net/) or [PuTTY](http://www.putty.org/) instead of using `minikube ssh` when asked in the labs to make it easier to edit command lines (or you will potentially run into the impossibility of correcting the shell commands you are about to type)
- To configure access to the `minikube` VM via _MobaXterm_ or _PuTTY_:
  - launch `minikube ssh-key` to view the path to the ssh key to use to connect to the VM
  - convert the key to the format expected by `PuTTY`/`MobaXterm`
  - launch `minikube ip` to know the ip of the VM `minikube`
  - create a new connection with this information (username: `docker`)

<div class="pb"></div>
