# Multifractal analysis of Turkish stock market

In this project, i will work on multifractal structure of Turkish stock market. Theoretically, financial markets are assumed to be efficient in most cases. However, it may not be realistic assumption many times. In this project, I will apply multifractal detrended fluctuation analysis (MFDFA) analysis of Kantelhardt et al. (2002) to test market efficientcy in BIST100 index.

Multifractal detrended fluctuation analysis (MFDFA) is a method for analyzing the scaling properties of non-stationary time series. It is a generalization of detrended fluctuation analysis (DFA), which is a widely used method for analyzing the scaling properties of stationary time series. It has been used to study a variety of time series, including financial data, climate data, and biological data.  

MFDFA works by first detrending the time series by fitting a polynomial to it. The detrended time series is then divided into overlapping windows, and the root mean square (RMS) of the fluctuations within each window is calculated. The RMS values are then plotted as a function of the window size.

If the time series is monofractal, the RMS values will follow a power law relationship with the window size. The exponent of the power law is known as the Hurst exponent, and it measures the degree of long-range correlations in the time series.

If the time series is multifractal, the RMS values will not follow a single power law relationship. Instead, they will follow a power law relationship with different exponents for different values of the q-parameter. The q-parameter measures the scale dependence of the fluctuations in the time series.

References:
Kantelhardt, J. W., Zschiegner, S. A., Koscielny-Bunde, E., Havlin, S., Bunde, A., & Stanley, H. E. (2002). Multifractal detrended fluctuation analysis of nonstationary time series. Physica A: Statistical Mechanics and its Applications, 316(1-4), 87-114.
