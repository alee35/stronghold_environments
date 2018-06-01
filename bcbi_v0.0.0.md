# BCBI_v0.0.0

This is BCBI's base environment. It was created from sctach

## Outline

* Set up environment
	* Create empty environment
	* Install basic system libraries
	* Python
	* R
	* Julia
* Trouble shooting comments

## Set up environment

### Create Environment
```
conda create --name bcbi_v0.0.0 python=3.6
source activate bcbi_v0.0.0
```

### System Basics: 
```
conda install gcc git hdf5
```

### Python packages

Basics

```
conda install numpy scipy matplotlib jupyter seaborn pandas
```

:warning: use libgcc from brown-datascience channel. Julia 


Others
```
conda install scikit-learn statsmodels
conda install -c conda-forge plotnine xgboost spacy
```

### R
```
conda install -c r r-base
conda install -c r r-caret r-randomforest r-lme4 r-dplyr r-data.table r-ggplot2 r-tidyr r-stringr r-glmnet r-survival r-devtools r-car r-lubridate r-rmysql
conda install -c conda-forge r-ranger r-gbm
```

### Julia

```
conda install julia -c brown-data-science
```

Setup Julia environment variables: .juliarc.jl
```
export JULIA_PKGDIR="/opt/browncis/conda/envs/bcbi_v0.0.0/lib/julia/packages"
	mkdir opt/browncis/conda/envs/bcbi_v0.0.0/lib/julia/packages
```  
  
If .juliarc.jl doesn't exist
```
cd ~
touch .juliarc.jl
```


```
ENV["JULIA_PKGDIR"] = "/opt/browncis/conda/envs/$(ENV["CONDA_DEFAULT_ENV"])/lib/julia/packages"
ENV["PYTHON"] = "/opt/browncis/conda/envs/$(ENV["CONDA_DEFAULT_ENV"])/bin/python"
ENV["CONDA_JL_HOME"] = "/opt/browncis/conda/envs/$(ENV["CONDA_DEFAULT_ENV"])/lib/julia/packages/v0.6/Conda/deps/usr"
```
  
:exclamation: If using Python packages, add to juliarc.jl
This sets Julia to use environment's Python. 
Note this is important as path to conda and python changes from build server (/opt/browncis/conda -->/opt/conda) to workstation (/opt/browncis/conda)

In julia's REPL
```
Pkg.dir() #confirm location
Pkg.init()
```  
  
