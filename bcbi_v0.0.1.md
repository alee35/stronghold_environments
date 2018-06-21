
# BCBI_v0.0.1

Based on BCBI_v0.0.0. Adding support for [PredictMD.jl](https://github.com/bcbi/PredictMD.jl)

## Outline

* [Set up environment](#set-up)
	* [Create empty environment](#Create-Environment)
	* [Install dependency libraries](#Dependencies)
	* [Julia](#julia)
* Trouble shooting comments

## Set up

### Create environment from YML

```
conda env create -f bcbi_v0.0.1.yml
source activate bcbi_v0.0.1
```

### Set up environment variables

```
cp /opt/browncis/conda/envs/bcbi_v0.0.0/etc/conda/activate.d/set-usr-envs.s /opt/browncis/conda/envs/bcbi_v0.0.1/etc/conda/activate.d/set-usr-envs.sh
```

### ~Extra Dependencies~ 

```
conda install -c brown-data-science predictmd-pdf2svg predictmd-texlive
```

### Set up Julia Packages

#### Set up package directory
```
#Confirm package dir
Pkg.dir()

# Create dir (and path) to package directory
mkpath(Pkg.dir())

Pkg.init()
```

#### Install packages

```
Pkg.clone("https://github.com/bcbi/BCBI_base.jl.git")
Pkg.checkout("BCBI_base", "bcbi_v0.0.1")
```
