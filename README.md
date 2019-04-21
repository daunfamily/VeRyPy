![VeRyPy](doc/logo.png)

<!-- ```
V  R  P
Ve Ry Py
 easy
``` -->

<!-- TODO: check and update these badges from shield.io and travis-ci.org -->
<!-- TODO: upload to PyPI -->
[![PyPI version](https://badge.fury.io/py/flair.svg)](https://badge.fury.io/py/flair)
[![GitHub Issues](https://img.shields.io/github/issues/zalandoresearch/flair.svg)](https://github.com/zalandoresearch/flair/issues)
[![License: MIT](https://img.shields.io/badge/License-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)
<!-- TODO: https://www.smartfile.com/blog/testing-python-with-travis-ci/ -->
[![Travis](https://img.shields.io/travis/zalandoresearch/flair.svg)](https://travis-ci.org/zalandoresearch/flair)

VeRyPy is an **easy** to use **Python** library of classical Capacitated **Vehicle Routing Problem** (CVRP) algorithms. The enclosed implemented algorithms can be used to solve vehicle routing and travelling salesman problems (TSP). The code files are very loosely coupled so that you can take just the algorithms or the functionality you need in your studies or in your research project. 

Compared to the existing heuristic and metaheuristic open source VRP libraries such as the [VRPH](https://projects.coin-or.org/VRPH), [Google OR-Tools](https://developers.google.com/optimization/), and others, the focus in VeRyPy has been in reusability of the code and in replicating the existing results from the literature. The lightness of the framework and architecture of VeRyPy is very much intentional as many existing libraries are complex beasts to reason about and understand which limits their use in a more exploratory setting. Please note that the limitations of VeRyPy are also related to the main target of replication: it is not the fastest, the most sophisticated, nor the most effective library for solving these problems. However, if you are looking for something simple, VeRyPy might be just perfect fit.

An ensemble of relatively simple heuristics can be an effective and robust way to solve practical problems and learning on the effectivity of different approaches before rolling out a more specialized and sophisticated algorithm. Furthermore, the quality of the solutions produced by the state-of-the-art metaheuristics depend on the quality of the initial solutions and VeRyPy can be used to produce a varied set of initial solutions for these more advanced methods.

## Features

* Implementations of 15 CVRP heuristics that are:
  * deterministic
  * classical (from 60ies to mid 90ies, well-cited)
  * constructive (as opposed to improvement heuristic)
  * the correctness of implementation is shown through replication of the original results
  * tested on a comprehensive set of 454 well-known CVRP benchmark instances
* Collection of local search heuristics:
  * intra route: 2-opt, 3-opt, relocate, exchange
  * inter route: insert, 2-opt*, 3-opt*<sup>1</sup>, one-point-move, two-point-swap, redistribute, chain
* Wrappers for [LKH](http://akira.ruc.dk/~keld/research/LKH/), [ACOTSP](http://www.aco-metaheuristic.org/aco-code/public-software.html), and [Gurobi TSP](https://www.gurobi.com/documentation/8.1/examples/tsp_py.html) solvers
* Command Line user Interface (CLI) for using the library and the separate algorithms
* Integration, replication, and some unit tests
* Imports TSPLIB compatible CVRP and TSP files
* Exports VRPH compatible solutions 
* Visualizer for many of the 15 heuristics 
* Most algorithms are able to solve CVRP instances up to 1000 customers in under an hour
* The simpler algorithms can tackle problems with over 10 000 customers in a reasonable time

<sup>1</sup> In fact, due to its complexity, this just might be the only open source implementation of the 3-opt* operation out there.

<!-- TODO: animated gifs of the heuristics here -->
<!-- 
## Performance
TODO: Comparison to SOTA.
TODO: Replication results.
TODO: Time complexity curves from the paper
-->

## Quick Start

The command line use assumes TSPLIB formatted files (assuming VeRyPy is in your PYTHONPATH):
```bash
(base) python -O VeRyPy.py -a all E-n51-k5.vrp
```

> Note: running with `python -O` entirely disables `__debug__` and logging.

This simple Python code illustrates the API usage:
```python
import cvrp_io
from classic_heuristics.parallel_savings import parallel_savings_init
from util import sol2routes

E_n51_k5_path = r"E-n51-k5.vrp"

problem = cvrp_io.read_TSPLIB_CVRP(E_n51_k5_path)

solution = parallel_savings_init(
    D=problem.distance_matrix, 
    d=problem.customer_demands, 
    C=problem.capacity_constraint)

for route_idx, route in enumerate(sol2routes(solution)):
    print("Route #%d : %s"%(route_idx+1, route))
```

<!-- TODO: Make sure it works -->

<!-- TODO: A more comprehensive reference documentation can be found [here](/doc/). -->

### Dependencies and Installation

VeRyPy requires Python 2.7, NumPy, and SciPy. For CLI use you also need `natsort` from PyPI and some algorithms have additional dependencies: CMT79-2P, FR76-1PLT, GM74-SwRI and WH72-SwLS require `orderedset` from PyPI; MJ76-INS needs `llist` from PyPI; and FR76-1PLT, FG81-GAP, and DV89-MM require Gurobi with `gurobipy`. By default Be83-RFCS, SG82-LR3OPT, and Ty68-NN use LKH to solve TSPs, but they can be configured to use any other TSP solver (such as the internal one) if these external executables are not available.

<!-- TODO: insert dependency / object diagram here-->

## Contributing and Contacting

Please consider contributing to VeRyPy. But, if this library does not meet your needs, please check the alternatives: [VRPH](https://projects.coin-or.org/VRPH), [Google OR-Tools](https://developers.google.com/optimization/). In order to avoid reinventing (rewriting?) the wheel, please fight the NIH and consider the aforementioned options before rolling out your own VRP/TSP library.

All contributions including, but not limited to, improving the implemented algorithms, implementing new classical heuristics or metaheuristics, reporting bugs, fixing bugs, improving documentation, writing tutorials, or improving the usability of the library are welcome. However, please note that any contributions you make will be under the MIT Software License. Even if you are not that much into coding please note that blogging, tweeting, and just plain talking about VeRyPy will help the project to grow and prosper.

When contributing:
* Report bugs using Github issues, and, while doing so, be specific and show how to reproduce the bug
* Use a consistent Pythonic coding style (PEP 8)
* The comments and docstrings have to be written in NumPy Style

Feature requests can be made, but there is no guarantee that they will be addressed. For a more complex use cases one can contact Jussi Rasku who is the main author of this library and is currently working with the transportation planning startup NFleet: <jussi.rasku@nfleet.fi>

If you are itching to get started, please refer to the todo list below:

1. Package project : https://packaging.python.org/tutorials/packaging-projects/
2. Transform all docstrings to NumPy format 
3. Generate restructured documentation 	https://pythonhosted.org/sphinxcontrib-restbuilder/
https://www.sphinx-doc.org/en/master/usage/quickstart.html

## Citing VeRyPy

If you find VeRyPy useful in your research and use it to produce results for your publications please consider citing it as:

Rasku J, Musliu N, Kärkkäinen T. Meta-Survey and Implementations of Classic
Capacitated Vehicle Routing Heuristics with Reproductions of Earlier Results. Manuscript. University of Jyväskylä, Finland.

<!-- ## References
TODO: links -->

## License

VeRyPy is licensed under the MIT license.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.





 
