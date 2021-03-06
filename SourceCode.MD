```python
# -*- coding: utf-8 -*-
"""
Created on Fri Oct  9 20:45:33 2020

@author: Maneet Jain 
"""
from numpy import power as pow
from scipy.integrate import quad
F = float(input("Molar flow rate (mol/s): "))
X = float(input("Conversion: "))
k = float(input("Rate constant: "))
n = float(input("Order of reaction: "))
C0 = float(input("Intial Conc. (mol/l): "))

#%%
def integrate(x):
    return 1/(1-x)**n

#%% PFR
def PFR():
    ans,err = quad(integrate,0, X)
    V1 = (F/(k*pow(C0,n))) * ans
    return V1

#%% CSTR
def CSTR():
    C = C0*(1-X)
    r = -k*(pow(C,n))
    V2 = (F*X)/(-r)
    return V2

#%% Batch Reactor
def Batch():
    v = F/C0
    T = V1/v
    t = T
    ans,err = quad(integrate,0, X)
    V3 = (N/(t*k*pow(C0,n))) * ans
    return V3

#%%
#CSTR + PFR
def CSTR_PFR():
#CSTR
    C1 = C0*(1-X1)
    r = -k*(pow(C1,n))
    V4 = (F*X1)/(-r)
#PFR    
    ans,err = quad(integrate, X1, X2)
    V5 = (F/(k*pow(C1,n))) * ans
    
    print("\nResults:")
    print("Volume of CSTR is: " + str(format(V4, '.3f')) + " Litres &", end = ' ')
    print("Volume of PFR is: " + str(format(V5, '.3f')) + " Litres.\n")
    V6 = V4 + V5
    return V6

#%% PFR + CSTR
def PFR_CSTR():
#PFR
    ans,err = quad(integrate, 0, X1)
    V7 = (F/(k*pow(C0,n))) * ans
#CSTR    
    C1 = C0*(1-X1)
    C2 = C1*(1-X2)
    r = -k*(pow(C2,n))
    V8 = (F*(X2-X1))/(-r)
    
    print("\nResults:")
    print("Volume of PFR is: " + str(format(V7, '.3f')) + " Litres &", end = ' ')
    print("Volume of CSTR is: " + str(format(V8, '.3f')) + " Litres.\n")
    V9 = V7 + V8
    return V9

#%%
print("_____________________________________Solving for PFR_____________________________________")
V1 = PFR()
print("\nResults:")
print("Volume of the PFR is: " + str(format(V1, '.3f')) + " Litres.\n")

print("_____________________________________Solving for CSTR____________________________________")
V2 = CSTR()
print("\nResults:")
print("Volume of the CSTR is: " + str(format(V2, '.3f')) + " Litres.\n")

print("\n_______________________________Solving for Batch Reactor________________________________")
N = float(input("Moles of reactant (mol): "))
V3 = Batch()
print("\nResults:")
print("Volume of the Batch reactor is: " + str(format(V3, '.3f')) + " Litres.\n")

print("________________________________Solving for reactors in series___________________________")
X1 = float(input("Conversion at the end of reactor 1: "))

if X1<X: 
    X2 = X
    print("\n______________________________Solving for CSTR followed by PFR___________________________")
    V6 = CSTR_PFR()
    print("Overall volume of the system is: " + str(format(V6, '.3f')) + " Litres.\n")
    print("_______________________________Solving for PFR followed by CSTR__________________________")
    V9 = PFR_CSTR()
    print("Overall volume of the system is: " + str(format(V9, '.3f')) + " Litres.\n")
else:
    print("It can't be greater than the final conversion!")

print("________________________________________COMPARISON________________________________________\n")     
print("\tPFR ", "\t\tCSTR", "\t\tBatch Reactor ", "\t\tCSTR + PFR", end = ' ')
print("\t\tPFR + CSTR")
print("%8.3f %12.3f %15.3f %18.3f %15.3f" % (V1, V2, V3, V6, V9))

print("\n__________________________________________RESULT__________________________________________")
V10 = min(V1, V2, V3, V6, V9)
print("\nMinimum volume is: " + str(format(V10, '.3f')) + " Litres.")


if V10 == V1:
    print("PFR has the smallest volume!")    
if V10 == V2:
    print("CSTR has the smallest volume!")    
if V10 == V3:
    print("Batch reactor has the smallest volume!")        
if V10 == V6:
    print("CSTR followed by PFR has the smallest volume!")
if V10 == V9:
    print("PFR followed by CSTR has the smallest volume!")

```
