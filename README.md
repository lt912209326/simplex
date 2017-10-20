# Simplex Optimization Algorithm and Implemetation in C++

# Introduction
The downhill simplex algorithm was invented by Nelder and Mead [1]. It is a method to find the minimum of a function in more than one independent variable. The method only requires function evaluations, no derivatives. Thus make it a compelling optimization algorithm when analytic derivative formula is difficult to write out. In this article, I will discuss the simplex algorithm, provide source code and testing code in C++, show rich examples and applications. I will basically follow the spirit of reference [1], in addition, I try to explain the algorithm in a more "example-oriented" manner.

# Simplex Algorithm
The downhill simplex algorithm has a vivid geometrical natural interpretation. A simplex is a geometrical polytope which has n + 1 vertexes in a n-dimensional space, e.g. a line segment in 1-dimensional space, a triangle in a plane, a tetrahedron in a 3-dimensional space and so on. In most cases, the dimension of the space represents the number of independent parameters that need to be optimized in order to minimize the value of a function: f(takes a vector of n parameters), where n is the dimension of the space.

To carry out the algorithm, firstly, a set of n+1 points is picked in the n-dimensional space, so that an initial simplex could be constructed. Clearly, the simplex is a n by n+1 matrix, each column is a point (in fact, a vector of size n) in the n-dimensional space. The simplex consists of n+1 such vectors. The initial simplex is constructed to be non-degenerate.
Secondly, the target function f is evaluated for all the n+1 vertexes on the simplex. So we can sort the results and have f(X_1) < f(X_2) . . . < f(X_n) < f( X_{n+1}) . Notice that each X_k is a vector of size n. Because our goal is to minimize the f, so we define the worst point as Xw=X_{n+1} and the best point is X_1.

Thirdly, the algorithm iteratively updates the worst point Xw by four possible actions: 1) reflection, 2) expansion, 3) 1-dimensional contraction, 4) multiple contraction. The following Fig. 1 sketches the actions in a 3-D space with a tetrahedron as the simplex.

1) Reflection: reflects away Xw through the centroid Xg of the other n best points, to get a reflected point X_r.

2) Expansion: if the newly found reflected point is better than the existing best point X_1, then the simplex expands toward the newly found reflected point X_r.

3) Contraction: if the reflected point X_r is no better than the existing best point, then the simplex contracts along 1-D through the direction of Xw and Xg, from the worst point Xw.

4) Multiple contraction: if the newly found X_r is even worst than the existing worst point Xw, then the simplex contracts along all dimensions toward the existing best point X_1.

The optimal solution of X_1 could be found by iterating the above actions on the updated condition of the simplex at each step.

# Application 1: function minimization
1. Find function minimum. (the source code can be found in the zip file folder "function_demos"). I show two examples, one is the famous Rosenbrock function and the original paper [1] also use this one as a good testing function. Because Rosenbrock function has a slow convergence "valley" so it is a good candidate to test the performance for optimization algorithms. The following Fig2. shows the trajectory of X_1 and the final convergence. The convergence is achieved within 100 steps in the plot. (with a considerately strict termination criteria. ) 
The second function i show is a polynomial function. We can see how fast the convergence can happen comparing with the case in Rosenbrock function. The convergence is achieved within 70 steps in the plot. (with the same termination criteria. )

For both Rosenbrock and the polynomial function, unconstrained parameter range is assumed. If constrains are necessary, one way to approach is to transform the original parameter to be a periodic parameter and change the target function prototype accordingly. 

# Application 2: nonlinear-least-square fit

2. Nonlinear-Least-Square fit (nls-fit) (the source code and the data can be found in the zip file folder "nlsq_fit".) A well-known algorithm to do nls-fit is called Gauss-Newton algorithm. Some famous statistics tools have Gauss-Newton algorithm built-in. However, the algorithm requires the calculation of the "score vector", which is the partial derivative with respect to each parameters. Using the simplex algorithm, we can easily implement the nls-fit without worrying about if the original model has continuous derivative or not. The target function for nls-fit is the RSS(residual sum of squares). i.e. we have a function Y=X*beta, where X is the predictor. We have a set of measurement on Y --- Ym. Then RSS=sum{ (Y-Ym) *(Y-Ym) } and our goal is the find a beta to minimize the RSS. The following formula shows a function that nonlinearly relates to the variable "lambda". 

The parameters need to be found out are:

{ I_0, Nw, lambdar_r, M, Nd, h }. and the initial values are:

{1.0, 33.0, 445.0, 1.0, 55.0, 0.0001}.

Using the simplex algorithm, the fitted parameters are:

{23.61, 32.82, 446.44, 0.7892, 55.09, -0.00108}.

The data, initial curve and fitted curve are shown in the following Fig.4. And it can be seen that the fitted parameters enable the model to match the data very well, although the starting curve is far from the data. If the covariance of the fitted parameters is a must, there are a few ways to get it. i.e., one way is to assume asymptotic normality of the error and calculate the Hessian matrix; another way is to assume no particular distribution function, but repeatedly sample a portion of the measurement data to carry out the nlsq-fit, which is called bootstrap method. Calculating the covariance of the fitted parameters is out of the scope of this article. 

# Source code and flowchart
In the attachment of this article, source code, make file is provided. The platform is ubuntu, g++ 4.2.4. i use STL and only standard C++, so the code is portable. The simplex fitting method is called BT::Simplex, there are illustration and examples on how to use the BT::Simplex method. All the above plots are generated based upon the output results from BT::Simplex. Finally, i'd like to share a graph showing the algorithm flowchart.

[1]Nelder, J.A., and Mead, R. 1965, "A simplex Method for Function Minimization", Computer Journal, Vol. 7, pp. 308-313.

