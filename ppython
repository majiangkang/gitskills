Jupyter Notebook
Binomial_Lattice_Calls_Puts
Last Checkpoint: 09/27/2019
(autosaved)
Current Kernel Logo
Python 3 
File
Edit
View
Insert
Cell
Kernel
Widgets
Help

Binomial lattice Program
​
​
import numpy as np
from math import exp, sqrt, log
s0 = 20; strike = 20;tp = 1.0;rf = 0.06;q= 0.04;sigma = 0.30;nperiods = 3
Ocall = True; Otype = 'E'
def make_stuff(tp,nperiods, sigma, rf, q):
    dt= tp/nperiods
    u = exp(sigma*sqrt(dt))
    d = 1/u
    disc = exp(-rf*dt)
    p = (exp((rf-q)*dt) -d)/(u-d)
    pbar = 1-p
    return u, d, p, pbar, disc
​
u, d, p, pbar, disc = make_stuff(tp,nperiods, sigma, rf, q)
print(u, d, p, disc)
1.1891099436471448 0.8409651313930471 0.4760197513442101 0.9801986733067553
def make_stock_lattice(s0, u, d, nperiods):
    S = np.zeros((nperiods+1, nperiods+1))
    S[0,0] = s0
    for t in range(1, nperiods+1):
        S[0,t] = S[0, t-1]*u
        S[t,t] = S[t-1, t-1]*d
        
    for t in range(2, nperiods+1):
        for j in  range(1, t):
            S[j,t] = S[j-1, t]*d**2
    return S
S= make_stock_lattice(s0, u, d, nperiods)
print(S)        
[[20.         23.78219887 28.27964916 33.62761202]
 [ 0.         16.81930263 20.         23.78219887]
 [ 0.          0.         14.14444704 16.81930263]
 [ 0.          0.          0.         11.89498677]]
def make_option_lattice(S, strike, nperiods, p, pbar, disc, Otype = 'E', Ocall= True):
    C = np.zeros((nperiods+1, nperiods+1))
    # Get Boundary Values
    for j in range(0, nperiods+1):
        C[j, nperiods] = max(0, S[j, nperiods]- strike)
        if (Ocall == False):
            C[j, nperiods] = max(0, strike- S[j, nperiods])
        
    for t in range(nperiods-1, -1, -1):
        for j in range(0, t+1):
            C[j,t] = disc*(p*C[j, t+1]+ pbar*C[j+1, t+1])
            if (Otype == 'A' and Ocall == True):
                C[j,t] = max(C[j,t], S[j,t]-strike)
            if (Otype == 'A' and Ocall == False):
                    C[j, t] = max(C[j,t], strike-S[j,t])
            
    return C
Otype = 'E'; Ocall = True
C = make_option_lattice(S, strike, nperiods, p, pbar, disc, Otype, Ocall)
print(np.round(C,2))
    
[[ 2.65  4.78  8.3  13.63]
 [ 0.    0.82  1.76  3.78]
 [ 0.    0.    0.    0.  ]
 [ 0.    0.    0.    0.  ]]
def option_price(s0, strike, tp, rf, q, sigma, nperiods, Otype='E', Ocall= True):
    #Step1:
    u, d,p, pbar, disc = make_stuff(tp,nperiods, sigma, rf, q)
    #Step 2
    S = make_stock_lattice(s0, u, d, nperiods)
    #Step 3
    C = make_option_lattice(S, strike, nperiods, p, pbar, disc, Otype, Ocall)
    return C[0,0]
​
​
mycall = option_price(s0, strike, tp, rf, q, sigma, nperiods, Otype, Ocall)
print(mycall)
2.6530621686003197
def get_all_options(s0, strike, tp, rf, q, sigma, nperiods):
    EC = option_price(s0, strike, tp, rf, q, sigma, nperiods, 'E', True)
    AC = option_price(s0, strike, tp, rf, q, sigma, nperiods, 'A', True)
    EP = option_price(s0, strike, tp, rf, q, sigma, nperiods, 'E', False)
    AP = option_price(s0, strike, tp, rf, q, sigma, nperiods, 'A' , False)
    return(EC, AC, EP, AP)
​
ans = get_all_options(s0, strike, tp, rf, q, sigma, nperiods)
print('The price of an European call is  ', round(ans[0],3))
print('The price of an  American call is ', round(ans[1],3),'\n')
print('The price  of an European put is  ', round(ans[2],3))
print('The price of an American put is   ', round(ans[3],3))
The price of an European call is   2.653
The price of an  American call is  2.653 

The price  of an European put is   2.273
The price of an American put is    2.328
def sensitivity_nperiods(s0, strike, tp, rf, q, sigma):
    mylist = np.arange(2,100)
    n = len(mylist)
    myoptions = np.zeros(n)
    for i, nperiods in enumerate(mylist):
        myoptions[i] = option_price(s0, strike, tp, rf, q, sigma, nperiods)
    return mylist, myoptions
​
mylist, myoptions =  sensitivity_nperiods(s0, strike, tp, rf, q, sigma)       
import matplotlib.pyplot as plt
%matplotlib inline
plt.plot(mylist, myoptions)
[<matplotlib.lines.Line2D at 0x1f67314c2b0>]

​
