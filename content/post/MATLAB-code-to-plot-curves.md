+++
title = "MATLAB Code to Plot Discrete 2D Points as Curves"
date = "2016-03-30"
+++

Currently, I am working on developing heuristics for an optimization problem. An inaccurate heurisitc is useless heuristic.
Therefore, we need to show that our heuristics are both performant and accurate. Aside from checking metrics such as MAPE, MAE, and 
RSME, we wanted to show that the shape of the graph generated for the heuristic approximates the shape of the graph for the optimal.
Initially I tried just using *plot* to render the graph, leading to something like this (I am not using actual graphs from the paper
in progress):

![Poor rendering of the graph](/img/badgraph.png)

Well that isn't right :P.

The obvious solution is to here is to generate a linearly spaced vector of the domain and use that to help interpolate the codomain
to get a smooth curve. The interpolation can be done using the *spline* function that uses [cubic spline interpolation](http://www.physics.utah.edu/~detar/phys6720/handouts/cubic_spline/cubic_spline/node1.html)
to generate the codomain points. This is pretty easy to do in MATLAB:

```matlab
    x1 = % the initial vector for the manipulated (domain) variable
    x2 = % the initial vector for the responding (codomain) variable
    x1prime = linspace(min(x1), max(x1), 10 * ceil(max(x1) - min(x1)));
    x2prime = spline(x1, x2, x1prime);
    plot(x1prime, x2prime, 'b');
```

Using the above should generate something like this:
![good graph 1](/img/good1.png)

Much better :)

Now suppose that you want to render the discrete points on the same graph using a marker. That is pretty simple. We simply 
use *plot(x1, x2, marker);*. We can even write a function that handles that for us:

```matlab
    function plotcurve(x1, x2, linetype, color, marker)
        
        % accepts vectors that represent discrete points in 2d
        % draws sa smooth curve between those points
        % puts markers at discrete points
        figure;
        hold on;
        numpoints = 10 * ceil(max(x1) - min(x1));
        x1prime = linspace(min(x1), max(x1), numpoints);
        x2prime = spline(x1, x2, x1prime);
        plot(x1prime, x2prime, strcat(linetype, color));
        plot(x1, x2, strcat(color, marker));
        hold off;
        
    end
```

Using the above, we get something like this:
![good2](/img/good2.png)

Now suppose that we want to do this for a variable number of pairs of vectors. We can abstact *plotcurve* under a function
that accepts a variable number of arguments using *varargin*. *varargin* exposes a cell array that contains the arguments supplied to our function, and we can 
use *nargin* to determine the number of argument supplied. The contents of a cell array can be accessed using *cell_array{index}*. In conjunction with
*nargin*, this allows us to iterate over our arguments and supply them to *plotcurve*.

```matlab

function plotcurve(x1, x2, linetype, color, marker)
    
    % accepts vectors that represent discrete points in 2d
    % draws sa smooth curve between those points
    % puts markers at discrete points
    numpoints = 10 * ceil(max(x1) - min(x1));
    x1prime = linspace(min(x1), max(x1), numpoints);
    x2prime = spline(x1, x2, x1prime);
    plot(x1prime, x2prime, strcat(linetype, color));
    plot(x1, x2, strcat(color, marker));

end

function plotcurves(varargin)
    figure;
    hold on;
    defaultlinetype = '-';
    for idx = 1:4:nargin
        x1 = varargin{idx};
        x2 = varargin{idx + 1};
        color = varargin{idx + 2};
        marker = varargin{idx + 3};
        plotcurve(x1, x2, defaultlinetype, color, marker);
    end
    hold off;
end

```

We can then call as follows:

```matlab

    x1 = % defined
    x2 = % defined
    y1 = % defined
    y2 = % defined
    plotcurves(x1, x2, 'b', 'o', y1, y2, 'g', '*');

```

Which would generate a graph similar to:


![good3](/img/good3.png)

Code:

<script src="https://gist.github.com/InzamamRahaman/40598f0044a19a06324db1538bf36e5e.js"></script>


