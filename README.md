# Designing a Lead Compensator Calculator 
<img align="left" alt="COURSE" src="https://img.shields.io/badge/Course-ELE--639-brightgreen"/>
<img align="left" alt="COURSE" src="https://img.shields.io/badge/Language%20-Matlab-yellow"/>
<br>

## Description 
Knowledge gained from the course in control theory and experiments were applied to calculate the metrics of a Lead Compensator using MATLAB. Using the Lead function with the required arguments a model or an actual's transfer function and parameters can be found, along with their step response figures generated automatically. Below can be found the description on steps to calculate the transfer function and paramters.  

## Usage 
Required Inputs (in step): ess, PO, Tsettle, Trise.

The program solves a very specific type of problem where the design requirements for **compensated closed loop system** (i.e Bode plot and process transfer function) are known including the required **ess, PO, Tsettle, and Trise**. The input is also assumed to be **step**. 

`Lead(sys,pm,wcp,PO,ess, Ts,Tr)` 

## Motivation
ELE 639 - Control Systems Lab 

## Design and Steps to solve 
## **STEP 1** : Calculate the Model and Actual Uncompensated System Transfer Function and Specs. 
1. First, the program calculates **closed loop transfer function of uncompensated system, Gcl_u(s)** using the given process transfer function, G such that: Gcl_u = feedback(G,1,-1)
2. Then using the transfer function it calculates the actual specs: stepinfo(Gcl_u) (_or stepeval(Gcl_u,7) if file is available_). 
3. The model of the uncompensated system is generated using the zpk(Gcl_u) function and cancel the near-pole zeros and negligle pole. If it is a type 1 system, the Kdc would be 1 else the Kdc is calculated. 
4. Using the model function of uncompensated system, Gm_u(s) the model specs are calculated. 
5. Comparison between actual vs model is made. 

## **STEP 2**: Calculate the Controller Parameters: a0, a1, b0
1. a0 is calculated using the formula: **a0 = Kpos_c/Kpos_u**. That is, dividing the open loop gain of compensated by the open loop gain of uncompensated. 
2. Then the a1 and b1 values are found by first calculating the lift angle and then the magnitude of the compensated system. To do this, phase margin and wcp are required. 
3. The given PO value is used to determine the damping ratio, which is then used to calculate the phase margin. 
4. Further the given Tsettle is used in calculating the wcp of the system.
5. Then, using the calculated wcp, the phase and magnitude are found: **[m,p] = bode(sys,wcp)**. Also, the lift angle is calculated by the formula: **liftangle = -180 + phasemargin - phase**. Both the lift angle and magnitude are then plugged in the a1,b1 formulas. 
6. The parameters can then be plugged into the controller transfer function. 

## **STEP 3**: Calculate the Model and Actual Compensated System Transfer function and Specs
1. Once, the transfer function of the controller is known, the compensated open loop system transfer function is formed by multiplying the controller function(**Controller**) with the process transfer function(**Plant**).
2. Then, the closed loop compensated system function is generated using **feedback** function.
3. As mentioned above, the specs of the **actual compensated system** are measured using the stepinfo function.
4. Then, the model is generated by first converting the compensated function to zero-pole form and then cancelling the negligle poles and near pole-zeros. 
5. The specs for the model is again calculated using the stepinfo function. 

The program also generates necessary bode plots and step response. 

## Output 
-Figure 1 and Figure 2 gives the step response of uncompensated system 
-Figure 3 Compares Gmu(model) with Gcl 
-Figure 4 gives the bode plot of the open loop system
