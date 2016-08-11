+++
title = "Trying to predict daily market crop prices"
date = "2014-07-31T11:09:17"
+++


After giving the data analysis component of the project a rest for a few weeks to develop the the backend for our agrimarketwatcher app and vizualization app, I started to take another look at trying to predict daily prices. Some crops may be harvested and sold over a period of days. In such instances, farmers might find it useful to have estimates of the prices for at least the current and next day to help them decide when to sell their crops to yield the best profit.

Furthermore, I realized that I may have become too entrenched in the belief that the time series modelling techniques I read about would have behooved our problem. Breaking free of this tunnel vision, I decided to model our problem using more run-of-the-mill machine learning techniques without any additional manipulation aside from run-of-the-mill min-max normalization. Modelling our problem as a mapping from R^k to R, I simply ignored all missing data entries and all data entries without at least k preceding data entries. Doing this in R is not all difficult. We define a “good” index, i, in our vector as an index at which a real number resides and the values at i-1, i-2…., i-k are all real numbers. After writing functions to determine if an index is “good”, we then simply apply the function to all indices, and return the indices, we label as “good”. The R code to accomplish this is short and sweet thanks to R’s equivalents of the higher order functions map and filter: the apply family of functions (in this case only sapply) and by-boolean element selection. The code to accomplish this is below:

<div class="gist-for-robots"><script src="http://gist.github.com/9b13a9c99bf521c59dbd.js"></script></div> 

Given the sparsity of our data sets, in order to build a respectable training set, it might be a good idea for us to keep k small, so let’s choose three as our value for k

<div class="gist-for-robots"><script src="http://gist.github.com/93477a1cecc762be6cdd.js"></script></div> 

Moreover, if we are going to use a neural network, we need to normalize the data to help prevent saturation at the asymptotes of the  sigmoidal activation function. In order to evaluate the accuracy of our neural network’s results, we also need to be able “denormalize” the output.  To make normalization and “denormalization” simpler,  it makes sense to define a higher-order function that, given a data set, can normalize that data set using min-max normalization  and “denormalize” the data set to which a particular min-max normalization was applied. This way, we don’t need to rewrite different functions to handle different data sets. Once again a pretty simple task.

<div class="gist-for-robots"><script src="http://gist.github.com/b602ecdca8e3264afca4.js"></script></div> 

Now that we have completed our set-up, we can actually define and evaluate our models.

<div class="gist-for-robots"><script src="http://gist.github.com/ec9d222486e1f3887b19.js"></script></div> 

Evaluating the models, we have the following results:

 

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;border-color:#bbb;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#bbb;color:#594F4F;background-color:#E0FFEB;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#bbb;color:#493F3F;background-color:#9DE0AD;}
</style><table class="tg"><tbody><tr><th class="tg-031e">Model\Metric</th><th class="tg-031e">MAPE</th><th class="tg-031e">MSE</th><th class="tg-031e">MAE</th></tr><tr><td class="tg-031e">Linear Model</td><td class="tg-031e">4.23</td><td class="tg-031e">0.49</td><td class="tg-031e">0.43</td></tr><tr><td class="tg-031e">SVM with Polynomial Kernel</td><td class="tg-031e">4.12</td><td class="tg-031e">0.54</td><td class="tg-031e">0.43</td></tr><tr><td class="tg-031e">SVM with Radial Basis kernel</td><td class="tg-031e">3.67</td><td class="tg-031e">0.42</td><td class="tg-031e">0.39</td></tr><tr><td class="tg-031e">Neural Network</td><td class="tg-031e">4.45</td><td class="tg-031e">0.43</td><td class="tg-031e">0.47</td></tr></tbody></table>
