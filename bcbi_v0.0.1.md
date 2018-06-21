
# BCBI_v0.0.1

Based on BCBI_v0.0.0. Added support for [PredictMD.jl](https://github.com/bcbi/PredictMD.jl)

## Outline

* [Set up environment](#set-up)
	* [Create empty environment](#Create-Environment)
	* [Install dependency libraries](#Dependencies)
	* [Julia](#julia)
* Trouble shooting comments

## Set up

### Create environments

```
conda create --name bcbi_v0.0.1 --clone bcbi_v0.0.0
source activate bcbi_v0.0.1
```

### Dependecies 

```
conda install -c brown-data-science predictmd-imagemagick predictmd-pdf2svg predictmd-texlive
```
