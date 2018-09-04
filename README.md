# Stronghold Environments

Documentation of steps followed to build/install packages in stronghold environments. This documentation should aid creating and updating new environments.

## Outline
* Basics
  * [Conda commands](#conda-commands)
  * [Screen commands](#screen-commands)
  * [Deploying evironments a.k.a sync to workstations](#deploy-environment)
  * [Pymodule on workstations](#pymodules)
* Environments
  * BCBI
    * [bcbi_v0.0.2-alpha](https://github.com/brown-data-science/stronghold_environments/blob/master/bcbi_v0.0.2-alpha.md)
    * [bcbi_v0.0.1](https://github.com/brown-data-science/stronghold_environments/blob/master/bcbi_v0.0.1.md)
    * [bcbi_v0.0.0](https://github.com/brown-data-science/stronghold_environments/blob/master/bcbi_v0.0.0.md)

## Conda commands

* List environments: `conda info --envs`
* List all packages in env with name `myenv`: `conda list -n myenv`
* Create environment with name `myenv` and specify python version: `conda env create -n myenv python=3.4`
* Create environment from yaml file: `conda env create -n myenv -f myenv.yml`
* Clone a previous environment: `conda create --name bcbi_v0.0.2 --clone bcbi_v0.0.1`
* Activate environments: `source activate bcbi_v0.0.2`
* Deactivate: `source deactivate`
* Remove environment with name `myenv`: `conda remove --name myenv --all`


## Screen commands

To avoid time outs, you may prefer to work in a screen session. Coomon commands are:

* start a new screen session with session name: `screen -S <name>`
* list running sessions/screens: `screen -ls`
* attach to a running session: `screen -x`
* attach to session by name: `screen -r <name>`  
* detach: `C-a d`
* detach and logout (quick exit): `C-a D D`  
* quit: `C-a :quit`

## Deploy Environment

Conda environments are created and set up on *build server*. However, they get distrubuted to the workstations as Pymodules via rsync. To make your environment available to the workstations follow these steps:

1. Register it as a PyModule and add related environment variables by editing `/opt/browncis/conda/conda` on *build server*

2. Update the pymodules database: `moduledb insert -f /opt/browncis/conda/conda` on *build server*

3. Now sync environments and pymodules database. This may take a while

 ```
 ssh admin server
 ssh software server
 #(software server may time out before sync is complete)
 screen -S screen_name
 #sync environment and pymodule settings
 sudo condasync
 #may want to use anoth buffer ctrl-a c (ctrl-a ctrl-a)
 sudo swsync
 ```

## PyModules

Confirm/load your module in stronghold workstation

* List: `module avail`
* Load: `module load conda/env_name`
