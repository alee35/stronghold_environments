
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
```

### Set up environment variables

The following will add environment to conda's activate script.

```
cd /opt/browncis/conda/envs/bcbi_v0.0.1/etc/conda/activate.d
touch set-usr-envs.sh
echo "export LD_LIBRARY_PATH=/opt/browncis/conda/envs/$CONDA_DEFAULT_ENV/lib" > set-usr-envs.sh
echo "export LD_LIBRARY_PATH=/opt/browncis/conda/envs/$CONDA_DEFAULT_ENV/lib" > set-usr-envs.sh
echo "export JULIA_PKGDIR=/opt/browncis/conda/envs/$CONDA_DEFAULT_ENV/lib/julia/packages" > set-usr-envs.sh
echo "export PYTHON=/opt/browncis/conda/envs/$CONDA_DEFAULT_ENV/bin/python" > set-usr-envs.sh
echo "export CONDA_JL_HOME=/opt/browncis/conda/envs/$CONDA_DEFAULT_ENV/lib/julia/packages/v0.6/Conda/deps/usr" > set-usr-envs.sh
echo "export MAGICK_HOME=/opt/browncis/conda/envs/$CONDA_DEFAULT_ENV/lib" > set-usr-envs.sh

source activate bcbi_v0.0.1
```

:warning: Make sure environment variables are properly set up before activating the environment. And double check their paths after activating the environment

### Set up Julia Packages

#### Set up package directory

```
#Confirm package dir
Pkg.dir()

# Create dir (and path) to package directory
mkpath(Pkg.dir())

Pkg.init()
```

#### Workarounds for failing installations

##### Rmath
In Julia's REPL

```julia
Pkg.add("Rmath")
```

:warning: Precompiling package will throw an error. A temporary fix, which is mentioned [here](https://github.com/JuliaStats/Rmath.jl/issues/40) is to run

```bash
make -C $JULIA_PKGDIR/v0.6/Rmath/deps/src/Rmath-julia-0.2.0/ CFLAGS=-v 
```

Re-build in julia

```julia
Pkg.build("Rmath")
````

##### HttpParser

```julia
Pkg.add("HttpParser")
```

:warning: Precompiling package will throw an error. A temporary fix, which is mentioned [here](https://github.com/JuliaWeb/HttpParser.jl/issues/75) is
to comment out the -Werror switch in `$JULIA_PKGDIR/v0.6/HttpParser/deps/src/http-parser-2.7.1/Makefile` line 57:

```
CFLAGS += -Wall -Wextra #-WerrorThis
```

Re-build in julia

```julia
Pkg.build("HttpParser")
```

#### Get BCBI_base packages

Back to Julia's REPL

```julia
Pkg.clone("https://github.com/bcbi/BCBI_base.jl.git")
Pkg.checkout("BCBI_base", "bcbi_v0.0.1")
using BCBI_base

install_all()
check_installed()
```

:warning: In the build server, "/opt/browncis/conda" is a sym-link tp "/opt/conda". All packages that use BinDeps.jl will write
a deps.jl file pointing to the later path. However, in the workstations only "/opt/browncis/conda" exists. Therefore, we run the following post-process command included in `postprocess_julia_deps.sh` to change all paths encountered in relevant files

```
find $JULIA_PKGDIR/v0.6 -name "deps.jl" -type f -exec sed -i "s+/opt/conda/envs/$CONDA_DEFAULT_ENV+/opt/browncis/conda/envs/$CONDA_DEFAULT_ENV+g" {} +
```

