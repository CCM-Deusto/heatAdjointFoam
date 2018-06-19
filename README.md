# Interior Control of the Heat Equation with the Steepest Descent Method

This is an OpenFOAM solver for the distributed control of the heat equation through the minimization problem

<p align="center">
  <img src="https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign*%7D%20%5Cmin_%7Bu%20%5Cin%20L%5E2%20%5Cleft%28%20%5COmega%20%5Ctimes%20%5Cleft%28%200%20%2C%20T%20%5Cright%29%20%5Cright%29%7D%20%5Cmathcal%7BJ%7D%20%5Cleft%28%20u%20%2C%20y%20%5Cleft%28%20u%20%5Cright%29%20%5Cright%29%20%3D%20%26%20%5Cmin_%7Bu%20%5Cin%20L%5E2%20%5Cleft%28%20%5COmega%20%5Ctimes%20%5Cleft%28%200%20%2C%20T%20%5Cright%29%20%5Cright%29%7D%20%5Cfrac%7B%5Cbeta_1%7D%7B2%7D%20%5Cint_0%5ET%20%5Cint_%7B%5COmega%7D%20u%5E2%20%5Cmathrm%7Bd%7D%20%5COmega%20%5C%2C%20%5Cmathrm%7Bd%7Dt%20&plus;%20%5Cfrac%7B%5Cbeta_2%7D%7B2%7D%5Cint_0%5ET%20%5Cint_%7B%5COmega%7D%20%5Cleft%28%20y%20-%20y_d%20%5Cright%29%5E2%20%5Cmathrm%7Bd%7D%20%5COmega%20%5C%2C%20%5Cmathrm%7Bd%7Dt%20&plus;%20%5Cfrac%7B%5Cbeta_3%7D%7B2%7D%20%5Cint_%7B%5COmega%7D%20%5Cleft%28%20y%5Cleft%28%20%5Ccdot%2C%20T%20%5Cright%29%20-%20Y_d%20%5Cright%29%5E2%20%5Cmathrm%7Bd%7D%20%5COmega%2C%20%5Cquad%20%5Cbeta_1%2C%20%5C%20%5Cbeta_2%2C%20%5C%20%5Cbeta_3%20%5Cin%20%5Cmathbb%7BR%7D%5E&plus;%2C%20%5Cend%7Balign*%7D">
</p>

subject to the state equation

<p align="center">
    <img src="https://latex.codecogs.com/gif.latex?%5Cbegin%7Bcases%7D%20%5Cpartial_t%20y%20-%20%5CDelta%20y%20%3D%20f%20&plus;%20u%20%26%20%5Ctext%7Bin%20%7D%20Q%20%3D%20%5COmega%20%5Ctimes%20%5Cleft%28%200%2C%20T%20%5Cright%29%2C%5C%5C%20y%20%3D%20y_D%20%26%20%5Ctext%7Bon%20%7D%20%5CSigma_D%20%3D%20%5CGamma_D%20%5Ctimes%20%5Cleft%28%200%2C%20T%20%5Cright%29%2C%5C%5C%20%5Cdisplaystyle%20%5Cfrac%7B%5Cpartial%20y%7D%7B%5Cpartial%20n%7D%20%3D%20y_N%20%26%20%5Ctext%7Bon%20%7D%20%5CSigma_N%20%3D%20%5CGamma_N%20%5Ctimes%20%5Cleft%28%200%2C%20T%20%5Cright%29%2C%5C%5C%20y%20%5Cleft%28%20%5Ccdot%2C%200%20%5Cright%29%20%3D%20y_0%20%26%20%5Ctext%7Bin%20%7D%20%5COmega%2C%20%5Cend%7Bcases%7D">
</p>

with <img src="https://latex.codecogs.com/gif.latex?%5CGamma%20%3D%20%5CGamma_D%20%5Ccup%20%5CGamma_N"> and <img src ="https://latex.codecogs.com/gif.latex?%5CGamma_D%20%5Ccap%20%5CGamma_N%3D%20%5Cemptyset">.

The cost functional gradient is

<p align="center">
    <img src="https://latex.codecogs.com/gif.latex?%5Cmathcal%7BJ%7D%5E%5Cprime%20%5Cleft%28%20u%20%5Cright%29%20%3D%20%5Cvarphi%20&plus;%20%5Cbeta_1%20u%2C">
</p>

where <img src="https://latex.codecogs.com/gif.latex?%5Cvarphi"> solves the adjoint problem

<p align="center">
    <img src="https://latex.codecogs.com/gif.latex?%5Cbegin%7Bcases%7D%20-%5Cpartial_t%20%5Cvarphi%20-%20%5CDelta%20%5Cvarphi%20%3D%20%5Cbeta_2%20%5Cleft%28%20y%20-%20y_d%20%5Cright%29%2C%20%26%20%5Ctext%7Bin%20%7D%20Q%2C%20%5C%5C%20%5Cvarphi%5Cleft%28%20%5Ccdot%2C%20T%20%5Cright%29%20%3D%20%5Cbeta_3%20%5Cleft%28%20y%20%5Cleft%28%20%5Ccdot%2C%20T%20%5Cright%29%20-%20Y_d%20%5Cright%29%2C%20%26%20%5Ctext%7Bin%20%7D%20%5COmega%2C%20%5C%5C%20%5Cvarphi%20%3D%200%2C%20%26%20%5Ctext%7Bon%20%7D%20%5CSigma_D%2C%20%5C%5C%20%5Cdisplaystyle%20%5Cfrac%7B%5Cpartial%20%5Cvarphi%7D%7B%5Cpartial%20n%7D%20%3D%200%2C%20%26%20%5Ctext%7Bon%20%7D%20%5CSigma_N.%20%5Cend%7Bcases%7D">
</p>

In the steepest descent method the cost gradient is used to update the control as

<p align="center">
    <img src="https://latex.codecogs.com/gif.latex?u%5E%7B%5Cleft%28%20n%20&plus;%201%20%5Cright%29%7D%20%3D%20u%5E%7B%5Cleft%28%20n%20%5Cright%29%7D%20-%20%5Cepsilon%20%5Cmathcal%7BJ%7D%5E%5Cprime%20%5Cleft%28%20u%5E%7B%5Cleft%28%20n%20%5Cright%29%7D%20%5Cright%29%2C%20%5Cquad%20%5Cepsilon%20%5Cin%20%5Cmathbb%7BR%7D%5E&plus;%2C">
</p>

with <img src="https://latex.codecogs.com/gif.latex?%5Cepsilon"> sufficiently small.

## Getting Started

The solver must be compiled in the terminal. It is advisable to first clean previous compilations with

```
wclean
```

and then use

```
wmake
```

### Prerequisites

OpenFOAM C++ library must be installed in order to compile the code.

The OpenFOAM distribution provided by the [OpenFOAM Foundation](https://openfoam.org/) was used.

## Running a Case

In order to run the solver move to the case directory and type in the command line

```
heatAdjointFoam
```

## Author

* **Jose Lorenzo Gomez**
* **Víctor Hernández-Santamaría**

## Acknowledgments

This project has received funding from the European Research Council (ERC) under the European  Union’s Horizon 2020 research and innovation programme (grant agreement No. 694126-DyCon).
 
[DyCon Webpage](http://cmc.deusto.eus/dycon/)
