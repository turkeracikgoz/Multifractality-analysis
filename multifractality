import numpy as np
import matplotlib.pyplot as plt

######################################################################
# This part checks if there is a problem with your parameters and data. 
#####################################################################
def check_time_series(data):
    """
    Checks whether time series data is appropriate for analysis.
    It should be a list or numpy array, containing more than one value. Besides, its should not be a constant vector.
    
    Parameters
    ----------
    data: list or np.ndarray
        A 1-dimensional timeseries of length "N".
    
    Raises
    ------
    ValueError: If the data is not of a valid type or has zero variance.    
    """
    
    if not isinstance(data, (list, np.ndarray)) or np.var(data) == 0:
        raise ValueError("Check your data!! It is either constant or in a inappropriate form")



def check_poly_ord(poly_ord):
    """
    Checks whether polynomial order for detrending is valid. It should be an integer.
    
    Parameters
    ---------
    poly_ord: int
            Order of polynomial fitting for detrending
    
    Raises
    ------
    
    ValueError: If the polynomial order is not an integer.
    """
    
    if poly_ord % 1 != 0:
        raise ValueError("Polynomial order must be an integer!!")


def check_scale(scale):
    if not isinstance(scale, (list, np.ndarray)) or len(scale) <= 1 or np.any(scale % 1 != 0):
        raise ValueError("scale must be a vector containing integers")
        
    """
    Checks if the window size variable "scale" is appropriate. It should be a numpy array or list of integers.
    
    Parameters
    ---------
    scale: list or np.ndarray
            Scale parameter for determining window size.
    
    Raises
    ------
    
    ValueError:  If the scale is not a list or numpy array, has less than two elements, or contains non-integer values.
    """

def check_q(q):
    """
    Checks whether q parameter is appropriate or not.
    
    Parameters
    ----------
    q: int, list or np.ndarray
        q-power parameter for fluctuation function.
        
    Raises
    ------
        ValueError: If the q parameter is not of a valid type or contains non-integer values.
    """
    
    if not isinstance(q, (int, list, np.ndarray)) or np.any(q % 1 != 0):
        raise ValueError("q must contain integers")


######################################################################
# Multifractal Detrended Fluctuation Analysis algorithm
#####################################################################
def multifractality(data, scale, q=2, poly_ord=1, Fig=1): 
    """
    Performs Multifractal Detrended Fluctuation Analysis on a univariate time-series data.
    
    Parameters
    ----------
    data: np.ndarray or list
        A numpy array or list, contains time series data.
    
    scale: np.ndarray of ints
            An array of integers to calculate segment sizes over.
            The minimum element of scale should be greater than polynomial order + 1 since polynomial fit of order k needs at least `k` points.
    
    q: np.ndarray of ints
        Fractal exponent to calculate. Default value is 2 for Detrended Fluctuation Analysis of monofractal series.
    
    order: int
        Polynomial degree for eliminating local trends. Default value is 1. Use 1 for least squares fitting and 2 for quadratic and 3 for cubic-fitting,
        which depends on the structure of profile.
    
    Fig: boolean
        A boolean variable for printing log(Fq) vs log(s). Default is 1. If Fig=1, then it prints the graph. 
    
    Returns
    -------
    Fq: np.ndarray
        an array of fluctuation function results depending on q-order and segment size, with shape (size(scale), size(q))
    
    Hq: np.ndarray
        an array of generalized Hurst exponents depending on q-order.
    """
    check_time_series(data)
    check_poly_ord(poly_ord)
    check_scale(scale)
    check_q(q)
    
    # Calculate the profile of univariate time series.
    
    X = np.cumsum(data - np.mean(data))
   
    # Be sure that scale > poly_order+1. To fit a polynomial with degree k, you need to have a data with length larger than k+1
    if np.min(scale) < poly_ord + 1:
        raise ValueError("The minimum scale must be larger than trend order poly_ord+1")
    
    # Define segment size as int(N/s) and repeat it for the remaining - tail - part of the series.
    segments = np.floor(len(X) / scale).astype(int)
    RMS_scale = {}
    qRMS = {}
    Fq = np.zeros((len(q), len(scale)))
    
    for s in range(len(scale)):
        RMS_scale[s] = np.zeros(segments[s])
        
        for v in range(segments[s]):
            # for each segment, fit a polynomial with order poly_ord and eliminate the local trends.
            # After that, obtain variance of residuals, Root mean square (RMS).
            Index = np.arange(v * scale[s], (v + 1) * scale[s])
            C = np.polyfit(Index, X[Index], poly_ord)
            fit = np.polyval(C, Index)
            RMS_scale[s][v] = np.sqrt(np.mean((X[Index] - fit) ** 2))
        
        for j in range(len(q)):
            # Define fluctuation function. If q=0, use logarithmic sums since $F_q(s)$ is divergent. 
            qRMS[(j, s)] = RMS_scale[s] ** q[j]
            Fq[j, s] = np.mean(qRMS[(j, s)]) ** (1 / q[j])
        Fq[q == 0, s] = np.exp(0.5 * np.mean(np.log(RMS_scale[s] ** 2)))
        
    
    Hq = np.zeros(len(q))
    line_q = {}
   
    for i in range(len(q)):
        # Regress log of window size, scale, on log of fluctuation function outputs.
        # The slope gives generalized Hurst exponent, $h_q$
        Y = np.polyfit(np.log2(scale), np.log2(Fq[i, :]), 1)
        Hq[i]=(Y[0])
        line_q[i] = np.polyval(C, np.log2(scale))
        
    ##############################
    # Plot the results. If the data, X(t), is long-range power law correlated, then $F_q(s)$ will increase for large values of s.
    # If the generalized Hurst exponent h(q) is constant for all q, then time series is mono-fractal. Otherwise, we say that data is multifractal.
    # Notice that h(2) is simply the Hurst exponent and reflects DFA results for monofractal series. Check the values of h(2) for persistency.
    # If h(2) = 0.5, data is random walk
    # When, h(2) > 0.5, we say that data is persistency exists. 
    # Otherwise, h(2) < 0.5 suggests the anti-persitent of this kind of fluctuations.
    ##############################
    
    if Fig == 1:
        qindex = [0, int(round(len(q) / 2)), len(q) - 1]
        qindex2 = [0, int(round((len(q) - 1) / 2)), len(q) - 2]
        
        qstart1 = q[qindex[0]]
        qmid1 = q[qindex[1]]
        qstop1 = q[qindex[2]]


        
        plt.figure()
        plt.plot(np.log2(scale), np.log2(Fq[qindex[0], :]), 'sb-', label='q = %.2f' % qstart1)
        plt.plot(np.log2(scale), np.log2(Fq[qindex[1], :]), 'og-', label='q = %.2f' % qmid1)
        plt.plot(np.log2(scale), np.log2(Fq[qindex[2], :]), 'vr-', label='q = %.2f' % qstop1)
        plt.plot(np.log2(scale), line_q[qindex[0]], 'b--')
        plt.plot(np.log2(scale), line_q[qindex[1]], 'g--')
        plt.plot(np.log2(scale), line_q[qindex[2]], 'r--')
        plt.xlabel('log2[scale]')
        plt.ylabel('log2[Fq]')
        plt.title('Multifractal Detrended Fluctuation Analysis')
        plt.legend()
   
    return Hq, Fq


############
Example
############
# Upload your data
data = np.loadtxt('dataset.txt') # my data
# define parameters
q=np.arange(-10,11,4) # vector of q values
poly_ord=1 # trend order.
max_scale=int(data.size/5)
scale=np.arange(10,max_scale,1) # vector of scales
# results
res=multifractality(data,scale,q,poly_ord,Fig=1)
Hq=res[0]
Fq=res[1]

for i in range(len(Hq)):
    print(f"Hurst Exponents for {q[i]:.0f} is {Hq[i]:.4f}")
