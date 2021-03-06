# hypervolume


Repository holds the code to repoduce the experiments detailed in:


Jonathan E. Fieldsend. 2019. 
Efficient Real-Time Hypervolume Estimation with Monotonically Reducing Error. 
In Genetic and Evolutionary Computation Conference (GECCO ’19), 
July 13–17, 2019, Prague, Czech Republic. ACM, New York, NY, USA

Institutional Repository: http://hdl.handle.net/10871/36825

Publisher DOI: https://doi.org/10.1145/3321707.3321730

The Main.java holds some of the orginal code used in the experiments in the paper, however this has now been deprecated as the codebase has been refactored, and the hypervolume estimator functionality has been pulled into seperate objects to reduce code duplication, and provide objects directly usable in other projects. As such the entry point for example usage is now ExampleGECCO.java

After compliation, running the class without arguments will detail what is expected. (Note the DLTZ2 cost function is used to illustrate the hypervolume estimators in this code.)

```
>> java hypervolume.ExampleGECCO 
Not enough input arguments, six arguments expected:
 Hypervolume estimate update type (B, I, S OR D),
 Number of samples compared per new estimate (B, I and S), or max nanoseconds for new samples (D_)
 number of iterations (minimum 0 applied),
 number of objectives (minumum 2 applied) and
 seed
 instrument? (true or false), if true hypervolume and timings are written to a file or each iteration
```

Some example runs would be 

```
java hypervolume.ExampleGECCO B 5000 100000 3 0 true
```

Which would use the basic MC sampling approach to hypervolume estimation, using 5000 samples per iteration of the (1+1)-ES optimiser for 100000 generations on the 3-objective problem varient. The seed used is 0 for the evolutionary optimiser, and outputs are written to a file.


```
java hypervolume.ExampleGECCO D 100000 50000 8 0 true
```

Which would use the dynamic MC sampling approach to hypervolume estimation, stopping hypervolume improvement estimatation at each iteration once 100000 nanonseconds has been dedicated to the estimation update. The (1+1)-ES optimiser will be run for 50000 generations on the 8-objective problem varient. The seed used is 0 for the evolutionary optimiser, and outputs are written to a file.

For more general use, the classs required are the implementors of the HypervolumeEstimator interface. In terms of computational efficiency, the real choice is between the EfficientIncrementalHypervolumeEstimator class (which limits the number of MC samples to use to refine the estimate each generation) and DynamicHypervolumeEstimator (which has a ceiling on time spent _after_ previously non-dominated MC samples are re-checked if the estimated Pareto set has changed.)

Currently, and matching the published work, the non-dominated set is stored in a Dominance Decision Tree

Oliver Schütze. 2003. 
A New Data Structure for the Nondominance Problem in Multi-objective Optimization. 
In International Conference on Evolutionary Multi-Criterion Optimization, EMO 2003 
Lecture Notes in Computer Science (LNCS), Vol. 2632. Springer, 509–518.

However other data structure implementations could be swapped in given the interface used. 
