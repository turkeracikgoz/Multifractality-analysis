# Multifractal analysis of Turkish stock market

In this project, i will work on multifractal structure of Turkish stock market. Theoretically, financial markets are assumed to be efficient in most cases. However, it may not be realistic assumption many times. In this project, I will apply multifractal detrended fluctuation analysis (MFDFA) analysis of Kantelhardt et al. (2002) to test market efficientcy in BIST100 index.

Multifractal detrended fluctuation analysis (MFDFA) is a method for analyzing the scaling properties of non-stationary time series. It is a generalization of detrended fluctuation analysis (DFA), which is a widely used method for analyzing the scaling properties of stationary time series. It has been used to study a variety of time series, including financial data, climate data, and biological data.

MFDFA works by first detrending the time series by fitting a polynomial to it. The detrended time series is then divided into overlapping windows, and the root mean square (RMS) of the fluctuations within each window is calculated. The RMS values are then plotted as a function of the window size.

If the time series is monofractal, the RMS values will follow a power law relationship with the window size. The exponent of the power law is known as the Hurst exponent, and it measures the degree of long-range correlations in the time series.

If the time series is multifractal, the RMS values will not follow a single power law relationship. Instead, they will follow a power law relationship with different exponents for different values of the q-parameter. The q-parameter measures the scale dependence of the fluctuations in the time series.

In financial data, if the series is monofractal, we can say that market is efficient and there is no long-range power-law correlation exist. Otherwise, the market is not efficient and long-range auto-correlations can be observed.

The steps of MFDFA analysis is given below.

Step 1) Let $x(t)$ be a univariate time series such that $t=1,2,3,...N$. Find the detrended profile of the process as follows (take cumulative sums):
$$X(k) = \sum_{t=1}^{k} [x(t)-\overline{x(t)}] , k=1,2,3,...N$$
where $\overline{x(t)}$ is the average over the sample period.

Step 2) Divide the $X(k)$ into $N_s = [N/s]$ non-overlapping segments with equal length s. If $N/s$ is not an integer, get rid of the remaining part and repeat the same procedure starting from end of the series. As a result, $2*N_s$ segments are obtained.

Step 3) Calculate the local trend for each of the $2*N_s$ segments by a least-square fit of the series. It is basically a detrending procedure for deterministic trend. Then obtain the residuals of this fitting, $X_{s}(i).

Step 4) Determine the variance:

$$ f^{2}(s,v) = \frac{1}{s} \sum_{i=1}^{s} X_{s}^{2} [(v-1)s+i] ,  v=1,2,...N_s$$

and also repeat it for $v=N_{s+1}, 2, ... 2*N_s$

Step 5) Obtain the qth-order fluctuation function by average over all segments

$$F_{q} (s) = [ \frac_{1}^{2N_s} \sum_{v=1}^{2N_s} [f^{2}(s,v)]^{\frac_{q}^{2}]^{1/q}$$




References:
Kantelhardt, J. W., Zschiegner, S. A., Koscielny-Bunde, E., Havlin, S., Bunde, A., & Stanley, H. E. (2002). Multifractal detrended fluctuation analysis of nonstationary time series. Physica A: Statistical Mechanics and its Applications, 316(1-4), 87-114.
