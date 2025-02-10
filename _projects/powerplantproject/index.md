---
layout: post
title: MAE 336 Power Plant Design Project 
description: This project analyzes the thermodynamic performance of a combined gas-steam power plant with a net power output of 280 MW. Using MATLAB simulations and real gas properties, we evaluated the effects of varying pressure ratios on mass flow rates and thermal efficiency. The study integrates the Brayton and Rankine cycles, accounting for non-ideal efficiencies of turbines, compressors, and heat exchangers. By solving energy balance equations and implementing numerical methods, we determined optimal operating conditions for maximizing efficiency while minimizing energy losses. The project also involved extensive coding for enthalpy interpolation, steam extraction calculations, and system optimization through parametric analysis.
skills:
  - Thermodynamic modeling
  - MATLAB programming
  - Brayton & Rankine cycle analysis
  - Energy balance equations
  - Real gas property calculations
  - Heat exchanger performance evaluation
  - Data visualization & efficiency optimization
  - CoolProp 

main-image: /powerplant.png
---
## Report 
[View the Report](https://drive.google.com/file/d/1sbge84Kj5VTLvma-6pCeo9gdYqKxBvJP/view?usp=sharing)
---
## Code Used

```cpp
Wnet=280000; % power (kW)
rp= linspace(10,20,20); %Pressure ratios
N_C=0.82; %isentropic efficiency of compressor
N_T=0.86; %isentropic efficiency for gas and steam turbine

%Enthalpies(h) at each state (kJ/kg):
h1=191.81; h2=192.6079; h3=720.87; h4=725.553;
h5=3069.3; h6=2730.354; h7=2186.31; h8=300.19;
h9=660.6885; h10=1161.07; h11=595.178; h12=421.26;

%Fraction of steam removed:
y=(h3-h2)./(h6-h2);

%Relative pressures(Pr):
Pr8=1.3860; %relative pressure at state 8
Pr9=rp.*Pr8; %state 9
Pr10=167.1; %state 10
Pr11=(1./rp)*Pr10; %state 11

%Table A-17 (h) and Relative pressures(Pr):
A_17=[492.74 7.824; 503.02 8.411; 513.32 9.031; ...];

%Interpolate h values:
for i=1:size(rp,2)
    for j=1:size(A_17,1)
        if Pr9(i)==A_17(j,2)
            h11s(i)=A_17(j,1);
        elseif Pr11(i)>A_17(j,2) && Pr9(i)<A_17(j+1,2)
            Prl=A_17(j,2);
            Prh=A_17(j+1,2);
            hl=A_17(j,1);
            hh=A_17(j+1,1);
            h9s=hl+(hh-hl)*((Pr9(i)-Prl)/(Prh-Prl));
        end
    end
end

% Display the table
disp('Table: Pressure Ratio, Mass Ratio, and Thermal Efficiency');
disp(table_results);
