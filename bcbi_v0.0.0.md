# BCBI_v0.0.0

This is BCBI's base environment. It was created from sctach

## Outline

* [Set up environment](#set-up)
	* [Create empty environment](#Create-Environment)
	* [Install basic system libraries](#system-basics)
	* [Python](#python)
	* [R](#r)
	* [Julia](#julia)
* Trouble shooting comments

## Set up

The environment was created and set up in pswbuild6cit.services.brown.edu

### Create Environment
```
conda create --name bcbi_v0.0.0 python=3.6
source activate bcbi_v0.0.0
```

### System Basics: 
```
conda install gcc git hdf5
```

:warning: Don't install libgcc from default. Instead used libgcc from brown-datascience channel (julia issues)

### Python

Basics

```
conda install numpy scipy matplotlib jupyter seaborn pandas
```

Others
```
conda install scikit-learn statsmodels
conda install -c conda-forge plotnine xgboost spacy
```

### R

```
conda install -c r r-base
conda install -c r r-caret r-randomforest r-lme4 r-dplyr r-data.table r-ggplot2 r-tidyr r-stringr r-glmnet r-survival r-devtools r-car r-lubridate r-rmysql
```


:warning: Tried `conda install -c conda-forge r-ranger r-gbm`. But that reverted most r-packages to use conda-forge version. Therefore decided not to install them

### Julia

```
conda install julia -c brown-data-science
```

### Setup Julia environment variables 

* Create a script that is read by `conda activate`

	```
	touch /opt/browncis/conda/envs/bcbi_v0.0.0/etc/conda/activate.d/set-usr-envs.sh
	```

* Add the following environment variables to that file

	1. `LD_LIBRARY_PATH`: This tell julia where to find binary dependencies i.e `R`
	```
	export LD_LIBRARY_PATH = /opt/browncis/conda/envs/$CONDA_DEFAULT_ENV/lib
	```
	2. `JULIA_PKGDIR`: Where all julia packages are downloaded to 
	```
	export JULIA_PKGDIR = /opt/browncis/conda/envs/$CONDA_DEFAULT_ENV/lib/julia/packages
	```

	3. `PYTHON`: Which python to use. We choose the default in the environment
	```
	export PYTHON = /opt/browncis/conda/envs/$CONDA_DEFAULT_ENV/bin/python
	```
	4. `CONDA_JL_HOME`: Where Conda lives 
	```
	export CONDA_JL_HOME = /opt/browncis/conda/envs/$CONDA_DEFAULT_ENV/lib/julia/packages/v0.6/Conda/deps/usr
	```  

:exclamation: Alternatively they could be set up in `~/.juliarc.jl` but then they would only be appropiately configured for your user

:warning: These environment variables also need to be the same in the workstation and therefore, when registering the environment as a PyModule you need to configure them. i.e in `/opt/conda/conda` the following block was added:

	 ```
	 [bcbi_v0.0.0]
	set JULIA_PKGDIR = %(rootdir)s/lib/julia/packages
	set JAVA_HOME = %(rootdir)s
	set PYTHON_DIR = %(rootdir)s/bin
	set PYTHON = %(rootdir)s/bin/python 
	set CONDA_JL_HOME = %(rootdir)s/lib/julia/packages/v0.6/Conda/deps/usr
	set LD_LIBRARY_PATH = %(rootdir)s/lib
	```
	
### Init Julia's package directory

In julia's REPL
```
Pkg.dir() #confirm location
Pkg.init()
```  

### Install BCBI's packages 

Follow [BCBI instructions](https://github.com/bcbi/BCBI_base.jl) and remember to refer to the appropiate tag.


