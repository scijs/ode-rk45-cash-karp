# ode-rk45-cash-karp [![Build Status](https://travis-ci.org/scijs/ode-rk45-cash-karp.svg)](https://travis-ci.org/scijs/ode-rk45-cash-karp) [![npm version](https://badge.fury.io/js/ode-rk45-cash-karp.svg)](http://badge.fury.io/js/ode-rk45-cash-karp) [![Dependency Status](https://david-dm.org/scijs/ode-rk45-cash-karp.svg)](https://david-dm.org/scijs/ode-rk45-cash-karp)

> Integrate a system of ODEs using the Fifth Order Adaptive Cash-Karp Runge-Kutta (RKCK) method


## Introduction

This module integrates a system of ordinary differential equations of the form

<p align="center"><img alt="&bsol;begin&lcub;eqnarray&midast;&rcub; y&apos;&lpar;t&rpar; &amp;&equals;&amp; f&lpar;t&comma; y&lpar;t&rpar;&rpar;&comma; &bsol;&bsol; y&lpar;t&lowbar;0&rpar; &amp;&equals;&amp; y&lowbar;0 &bsol;end&lcub;eqnarray&midast;&rcub;" valign="middle" src="docs/images/begineqnarray-yt-ft-yt-yt_0-y_0-endeqnarray-0298eae3db.png" width="187" height="61"></p>

where <img alt="y" valign="middle" src="docs/images/y-720f311276.png" width="14.5" height="20"> is a vector of length <img alt="n" valign="middle" src="docs/images/n-9baedbc330.png" width="16" height="16">. Given time step <img alt="&bsol;Delta t" valign="middle" src="docs/images/delta-t-a20a5fe4f2.png" width="28" height="16">, the Cash-Karp method uses a fifth order Runge-Kutta scheme with a fourth order embedded scheme in order to compute an error estimate.

## Install

```bash
$ npm install ode-rk45-cash-karp
```

## Example

```javascript
var rk45 = require('ode-rk45-cash-karp')

var deriv = function(dydt, y, t) {
  dydt[0] = -y[1]
  dydt[1] =  y[0]
}

var y0 = [1,0]
var n = 1000
var t0 = 0
var dt = 2.0 * Math.PI / n

var integrator = rk45( y0, deriv, t0, dt )

// Integrate 1000 steps:
integrator.steps(n)
```



## API

### `require('ode-rk45-cash-karp')( y0, deriv, t0, dt )`
**Arguments:**
- `y0`: an array or typed array containing initial conditions. This vector is updated in-place with each integrator step.
- `deriv`: a function that calculates the derivative. Format is `function( dydt, y, t )`. Inputs are current state `y` and current time `t`, output is the calculated derivative `dydt`.
- `t0`: initial time <img alt="t" valign="middle" src="docs/images/t-fc93da6f4d.png" width="11.5" height="16">.
- `dt`: time step <img alt="&bsol;Delta t" valign="middle" src="docs/images/delta-t-a20a5fe4f2.png" width="28" height="16">.

**Returns**:
Initialized integrator object.

**Properties:**
- `n`: dimension of `y0`.
- `y`: current state. Initialized as a shallow copy of input `y0`.
- `deriv`: function that calculates the derivative. Initialized from input. May be changed.
- `t`: current time, incremented by `dt` with each time step.
- `dt`: time step <img alt="&bsol;Delta t" valign="middle" src="docs/images/delta-t-a20a5fe4f2.png" width="28" height="16">. Initialized from input `dt`. May be changed.

**Methods:**
- `.step()`: takes a single step of the RK-4 integrator and stores the result in-place in the `y` property.
- `.steps( n )`: takes `n` steps of the RK-4 integrator, storing the result in-place in the `y` property.

## Credits

(c) 2015 Ricky Reusser. MIT License