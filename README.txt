
== Introduction ==

This package contains all the source code to reproduce the nummerical
experiments described in [1]. It contains a parallelized implementation
of the MCA generative model training algorithm.


If you have problems running the code, please contact
 Joerg Bornschein <bornschein@fias.uni-frankfurt.de>

== Overview ==

pulp/       - Python library/framework for MPI parallelized 
               EM-based algorithms. The MCA implementation
               is in pulp/em/camodels/mca_et.py

examples/   - Small example programs for the ulp library

gf-learning - Actual main program that performs EM based learing using 
               pulp.em
               
preproc/    - Scripts used to extract patches from the Van-Hateren
               database and to preprocess them usnig pseudo-whitening.

data/       - Traing data. This package does not include the training data
               but contains a script that can be used to doenload the datasets.

== Data Sets ==

The script "data/download.sh" will download 3 datasets:
  
 1) vanhateren-linear.h5 
 2) patches16.h5 patches of size 16x16, preprocessed as described in [1]
 3) patches20.h5 patches of size 20x20, preprocessed as described in [1]
 4) patches26.h5 oatches of size 26x26, preprocessed as described in [1]


== Software dependencies ==
 
 * Python (>= 2.6)
 * NumPy (reasonable recent)
 * SciPy (reasonable recent)
 * pytables (reasonable recent)
 * mpi4py (>= 1.2)

== Running ==

First, run some examples. E.g.

  $ cd examples
  $ python mca-barstest.py

This should run the MCA algorithm on artificaial bars data and 
visualize the result.


To run the real experiments change into the 'gf-learing/' directory and 
choose a parameter-file:

 $ python run-em.py params-16x16-h100.py

The results will be stored into a file called "output/.../results.h5".
This file contains the W, pi, sigma parameters for each EM iteration. To 
visualize these results you can use "./report <output-dir-name>", which
will save the generated plots into the "output/.../" directory again. 


Running on big datasets, with high values of H, Hprime and gamma is
computationally very expensive; use MPI to parallelize:

a) On a multi-core machine with 32 cores

 $ mpirun -np 32 python run-em.py params-h200.py

b) On a cluster:

 $ mpirun --hostfile machines python run-em.py params-h200.py

 where 'machines' contains a list of suitable machines.

See you MPI documentation for the details how to start MPI parallelized .


Using 160 parallel cores for the 16x16 patches, a single EM step takes about
1:30 minutes -- less 2h for a full EM training run. Running 26x26 takes much
longer (more than 24h).
The Hprime and gamma parameters have a strong influence on the computational
cost.


== References ==

[1] The Maximal Causes of Natural Scenes are Edge Filters,
     G. Puertas, J. Bornschein, J. Lucke, 
     Advances in Neural Information Processing Systems 23, 2010
