+++
title = "Experience at trying to predict crop prices"
date = "2014-07-17T08:45:13"
+++


Trinidad and Tobago, like most of the Caribbean is lacking in terms of food security. As a whole, every year, the Caribbean imports billions of USD of food. Since ICT has been so trans-formative in so many areas, it makes sense to try to apply it to help Trinidad and Tobago alleviate its food security dilemma.  To this end, those of us in the AgriNet project have thrown our proverbial hats in the ring to help tackle this important issue by developing technologies to assist farmers and policy makers.

I was assigned to the data analytics group of the project, and I was tasked with developing systems to help farmers decide when they should plant and sell their crops to yield the greatest revenue. Even though experienced farmers would possess some tacit knowledge of when their crops would fetch their highest prices during the year, we thought that it  would be useful for them to have some more  precise estimates within a reasonable time frame to make better financial decisions.

In order to achieve this, I started to perform analysis on the data collected and stored as xls files by NAMDEVCO, and unfortunately, despite my best attempts (I tried a lot of different things, most of which I wouldn’t mention here as it would take too long), results were poor.   Nevertheless, even failures can be edifying, and at the very least, maybe me recounting some bits of my experience can help someone else. Perhaps, one day, when there is more data or I have more experience and knowledge with machine learning, I can try my hand at it again, but for now, I’ll just present my most successful failure.

- - - - - -

 

At first I tried to consider predictions at the granularity of days. However, data proved to be a serious issue. It seems as though NAMDEVCO doesn’t collect data over  weekends. Moreover, there were instances where crops had an entry for the volumes brought to the market for a particular day, but the price data was missing. To the best of my current knowledge, time series analysis tends to require that data points are equally spaced across time. Upon trying to impute data over the two years for which daily data was recorded, I found some cases, such as ginger, where more that 50% of the needed data was missing – in the case of ginger 369 days worth of price data was missing. With so many gaps and inconsistencies in the data, we decided to abandon trying to predict prices accurately at the level of days.

We next tried at the level of months, which ,thankfully, had significantly less missing values. Moreover, at the level of months we could actually see trends. For example, below is a basic plot of the mean price of small limes:

<div class="wp-caption alignnone" id="attachment_18" style="width: 368px">[![monthly time series of mean lime prices ](http://kyledef.org/inzamam/wp-content/uploads/2014/07/limes-300x175.png)](http://kyledef.org/inzamam/wp-content/uploads/2014/07/limes.png)monthly time series of mean lime prices

</div> 

From the above plot, we can see that small lime prices reach their annual peak from about April to June and their annual nadir from about August to October. Using an acf plot, we can see that there is marked autocorrelation in the data:

[![acf-limes](http://kyledef.org/inzamam/wp-content/uploads/2014/07/acf-limes-300x175.png)](http://kyledef.org/inzamam/wp-content/uploads/2014/07/acf-limes.png)

I am no expert at time series analysis (or at anything else for that matter), but I thought I could’ve used an ARIMA model here. In order to evaluate my model I would of course need to reserve some of my data for testing purposes.

To this end, I wrote the following R code:

<div class="gist-for-robots"><script src="http://gist.github.com/09748cdb313860214504.js"></script></div>The arima model’s results were lacking. To remedy this, I used methods inspired by:  
[Combining Neural Networks and Time Series](http://www.google.tt/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CBwQFjAA&url=http%3A%2F%2Fwww.researchgate.net%2Fpublication%2F222679232_Combining_neural_network_model_with_seasonal_time_series_ARIMA_model%2Ffile%2Fe0b4952e3c3f235de6.pdf&ei=5gPIU7jxO6PLsATiwYH4DA&usg=AFQjCNGWntvGNUlyaIaesKT91qJJL-h4Zg&sig2=X7aH66SGZ1pPa9QGslcxpQ&bvm=bv.71198958,d.cWc)

I decided to use the arima model’s forecasts in conjunction with our historical data to make better predictions. I would then evaluate the models using their Mean Absolute Error, Mean Absolute Percentage Error,  Mean Square Error, and their noticeable deviations from the actual values.

<div class="gist-for-robots"><script src="http://gist.github.com/afa6dd95cde0fb33ce35.js"></script></div> 

Using the kernlab and neuralnet libraries, I then proceeded to construct and evaluate the following models:

1. Linear regression on Actual ~ (Zt * Z6 * Z12)^3
2. Support vector regression with a Radial Basis kernel function on Actual ~ (Zt * Z6 * Z12)^3
3. Support vector regression with a Polynomial kernel function on Actual ~ (Zt * Z6 * Z12)^3
4. A multilayer feed-foward neural network with a single hidden layer of two neurons on Actual ~ Zt + Z6 + Z12

where :

- Zt was the forecast for that month from the Arima model posited by auto.arima
- Z12 was the mean monthly price of the crop a year before our target time
- Z6 was the mean monthly price of the crop six months before our target time

<div class="gist-for-robots"><script src="http://gist.github.com/51635734fd751eca8e4a.js"></script></div>The above code yielded the following graph

[![Rplot](http://kyledef.org/inzamam/wp-content/uploads/2014/07/Rplot-300x175.png)](http://kyledef.org/inzamam/wp-content/uploads/2014/07/Rplot.png)

… and the following table to metrics with the values rounded to 2 decimal places. I am not happy with the results. Hopefully, I can do better next time

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;border-color:#bbb;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#bbb;color:#594F4F;background-color:#E0FFEB;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#bbb;color:#493F3F;background-color:#9DE0AD;}
</style><table class="tg"><tr><th class="tg-031e">Model\Metric</th><th class="tg-031e">MAPE</th><th class="tg-031e">MSE</th><th class="tg-031e">MAE</th></tr><tr><td class="tg-031e">1</td><td class="tg-031e">18.21</td><td class="tg-031e">274.42</td><td class="tg-031e">13.37</td></tr><tr><td class="tg-031e">2</td><td class="tg-031e">10.27</td><td class="tg-031e">105.25</td><td class="tg-031e">7.15</td></tr><tr><td class="tg-031e">3</td><td class="tg-031e">18.12</td><td class="tg-031e">267.66</td><td class="tg-031e">12.78</td></tr><tr><td class="tg-031e">4</td><td class="tg-031e">18.80</td><td class="tg-031e">244.28</td><td class="tg-031e">12.81</td></tr></table>
