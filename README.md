import numpy as np

t_initial = 0 #the current time

t_end = 200 #the end time of the simulation, number of days the simulation accounts for  

h = 0.1 # setting up the step size to use the Euler's method, the smaller the value, the more precise the solution
       # to the differential equation will be
steps = int((t_end - t_initial)/h + 1) # calculating the number of steps

 #Setting up the coefficients for the SIR model 
 
t = np.linspace(t_initial, t_end, steps) # creating a list of equal distributed storing t values
S = np.zeros(steps) # creating the list with zero values for storing S values
I = np.zeros(steps) # creating the list with zero values for storing I values
R = np.zeros(steps) # creating the list with zero values for storing R values

b = 0.050065 # setting up the b coefficient - infection rate, defined and justified in the cell above
c = 13.4 # setting up the c coefficient  - average contact rate, defined and justified in the cell above
e = 0.14 #setting up the re-infection rate of how many people become susceptible again
k = 0.0181   # setting up the k coefficient  - recovery rate, defined and justified in the cell above
T = 14 #days is the delay after which the person becomes susceptible again


# Assigning the current conditions of the susceptible and infected people 
N = 1428230000 #calculating the population size
S[0] = 1428095506/N
I[0] = 80565/N
R[0]= 53929/N


v = 10000/N # vaccination rate: proportion of population receiving shots per day

print(S[0],I[0],R[0])

def DSdt(S,I,R):
    # function puts in the values in the differential equation and returns the value of the function at given t, S, I
    return -b*c*S*I-v

def DIdt(S,I):
    #function puts in the values in the differential equation and returns the value of the function at given t, S, I
    return (b*c*S*I-k*I)
def DRdt (I):
    #function puts in the values in the differential equation and returns the value of the function at given t, I
    return k*I+v


for n in range(steps-1): # the for loop iterates over the number of steps to update the Values of S, I, and R 
                        # based on the virus
    S[n+1] = S[n] + h*DSdt(S[n],I[n],R[n]) #calcualting the values of S using the Euler's method
    I[n+1] = I[n] + h*DIdt(S[n],I[n]) #calcualting the values of I using the Euler's method
    R[n+1] = R[n] + h*DRdt(I[n]) #calcualting the values of R using the Euler's method

import matplotlib.pyplot as plt
%matplotlib inline
plt.rcParams.update({'font.size': 15})
plt.rcParams["figure.figsize"] = [8,5]
plt.plot(t,S,linewidth=2,label='S(t)')
plt.plot(t,I,linewidth=2,label='I(t)')
plt.plot(t,R,linewidth=2,label='R(t)')
plt.xlabel('t, (days)')
plt.ylabel('Proportion of the population')
plt.legend(loc='best')
plt.title("Development of the disease with no quarantine")
plt.show()

