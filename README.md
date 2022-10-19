# Transformers-Paper
Repo for the Paper presentation on Transformers.

## Overview
**Time Series Forecasting**

![img_2.png](assets/img_2.png)

* A variable that depends on time: stock price / day, number of hamburgers you eat / month,influenza cases / week.


* General nature of the problem: If you have past N datapoints, what's the next value going to be?
  * (hamburger_january, hamburger_february, hamburger_march) -> hamburger_april.


* Extremely common problem _(bread & butter of Wallstreet)_ and hosts of methods from different fields
  * AR (AutoRegression): Linear regression
  * ARMA (AR w/ Moving Average): Fancy Linear Regression #1
  * ARIMA (AR Integrated w/ MA ): Fancy Linear Regression #2
  * Exponential smoothing: Fancy Linear Regression #3
  * LSTMs etc.

**Motivating Question**

* Can you use Transformers for this?
  * Do Transformers even work with continuous data?
  * Does this problem require Transformers at all? AKA will you get better results?
  * What kind of Architecture?

### Introduction
**Problem Statement:** Predict ILIs (Influenza-Like Illness) in the future (Why?) using Transformers.

**Data:**  Weekly Country and State level ILI Ratio (influenza/total) published by CDC from 2010 to 2018

**Architecture**

![](assets/architecture.png)

Original Transformer (Encoder-Decoder) based architecture (Vaswani et al., 2017) with suitable modifications. [The algorithm can be found here.](Algorithm%20for%20Time%20Series%20Forecasting.pdf)

Compare this with the [original decoder algorithm here](assets/EDoriginal.png).

**Question1** 

What, in your opinion, is the most striking difference here? Would you say that the architectural differences are significant?

## Experiment
* Four encoder/decoder layers. Most of the other hyperparameters assumed to be default.
* Positional embeddings sine/cosine.
* Encoder Input is of the form (T1, T2, ..., T10)
* Decoder Input is of the form (T10, T11, ... T13)
* Prediction is made on (T11, ... T14)
* Minibatch size 64
* Adam Optimizer.
* Dropouts for all encoder/decoder layers with d=0.2
* Train/Test is 2:1
* Data is Min/Max scaled

## Results (for one-step-ahead forecasting)
![img.png](assets/img.png)

**Question 2**
* LSTM's pearson correlation is pretty good but RMSE, not so much. What do you think this implies?

Transformer performs better w.r.t baselines. But, for the state of the art (the-then) ARGONet, RMSE is slightly degraded (0.55 vs 0.59)

![img_1.png](img_1.png)

## Analysis
* Do we really need ED for this? This looks like a Decoder only Transformer.
* The entire point of embeddings is to compress N dimensional one-hot vectors to n<<N dimensional dense vectors. Aren't these already dense? Do we really need embeddings?
* The final combination layer is linear. Would sigmoid be better?
* 

## References
- Lu, F. S., Hattab, M. W., Clemente, C. L., Biggerstaff, M.,
and Santillana, M. Improved state-level influenza now casting in the united states leveraging internet-based data
and network approaches. Nature Communications, 10(1):
147, 2019. ISSN 2041-1723
- Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones,
L., Gomez, A. N., Kaiser, L. u., and Polosukhin, I. Attention is all you need. In Guyon, I., Luxburg, U. V., Bengio, S., Wallach, H., Fergus, R., Vishwanathan, S., and
Garnett, R. (eds.), Advances in Neural Information Processing Systems 30, pp. 5998–6008. Curran Associates,
Inc., 2017.

## Resources
- [Cdc fluview dashboard.](https://gis.cdc.gov/grasp/fluview/fluportaldashboard.html)
- [Time Series in Python — Exponential Smoothing and ARIMA processes](https://towardsdatascience.com/time-series-in-python-exponential-smoothing-and-arima-processes-2c67f2a52788)
- [Pearson's Correlation](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)







