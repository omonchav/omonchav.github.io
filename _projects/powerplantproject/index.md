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
h1=191.81; h2=192.6079; 
h3=720.87; h4=725.553; 
h5=3069.3; h6=2730.354; 
h7=2186.31; h8=300.19; 
h9=660.6885; h10=1161.07; 
h11=595.178; h12=421.26; 
%Fraction of steam removed: 
y=(h3-h2)./(h6-h2) 
%Relative pressures(Pr): 
Pr8=1.3860; %relative pressure at state 8  
Pr9=rp.*Pr8; %state 9 
Pr10=167.1; % state 10 
Pr11=(1./rp)*Pr10; %state 11 
%Table A-17 (h) and Relative pressures(Pr): 
A_17=[492.74 7.824 
503.02 8.411 
513.32 9.031  
523.63 9.684  
533.98 10.37 
544.35 11.10 
555.74 11.86 
565.17 12.66 
575.59 13.50 
586.04 14.38 
596.52 15.31 
607.02 16.28 
617.53 17.30 
628.07 18.36 
638.63 19.84 
649.22 20.64 
659.84 21.86 
670.47 23.13 
681.14 24.46 
691.82 25.85 
702.52 27.29 
713.27 28.80]; 
h9s=zeros(1,size(rp,2)); 
h11s=zeros(1,size(rp,2)); 
%Interpolate h values: 
for i=1:size(rp,2) 
for j=1:size(A_17,1) 
if Pr9(i)==A_17(j,2) 
h11s(i)=A_17(j,1); 
elseif Pr11(i)>A_17(j,2)&&Pr9(i)<A_17(j+1,2) 
Prl=A_17(j,2); 
Prh=A_17(j+1,2); 
hl=A_17(j,1); 
hh=A_17(j+1,1); 
h9s=hl+(hh-hl)*((Pr9(i)-Prl)/(Prh-Prl)); 
end 
end 
end 
for i=1:size(rp,2) 
for j=1:size(A_17,1) 
if Pr11(i)==A_17(j,2) 
h11s(i)=A_17(j,1); 
elseif Pr11(i)>A_17(j,2)&&Pr11(i)<A_17(j+1,2) 
Prl=A_17(j,2); 
Prh=A_17(j+1,2); 
hl=A_17(j,1); 
hh=A_17(j+1,1); 
h11s(i)=hl+(hh-hl)*((Pr11(i)-Prl)/(Prh-Prl)); 
break 
end 
end 
end 
%Turbine and compressor isentropic efficiencies: 
h9a=h8+((h9s-h8)./N_C); 
h11a=h10-(N_T.*(h10-h11s)); 
%Mass flow rate ratio of air to steam: 
M_rate=(h5-h4)./(h11a-h12); 
%Pump,Turbine and compressor Work: 
Wp1=0.7979; %pump 1 (kJ/kg) 
Wp2=4.683; %pump 2 (kJ/kg) 
QinR=h5-h4; %heat input for steam cycle 
QoutR=(1-y)*(h7-h1); %heat output for steam cycle 
WnetR=QinR-QoutR; %Net work for steam cycle (KW) 
WnetB=(h10-h11a)-(h9a-h8); %net work for gas cycle 
m_steam=Wnet./(WnetR+(M_rate.*WnetB)); % mass flow rate of steam (kg/s) 
m_air=M_rate.*m_steam; %mass flow rate of air 
WnetT=WnetR.*m_steam + WnetB.*m_air; %total net work of cycle (KW) 
%Rate of heat input: 
Qin=m_air.*(h10-h9a); 
%Thermal efficiency: 
N_thermal= Wnet./Qin; 
%Plot Mass Ratio vs Pressure ratio 
figure(1) 
plot(rp,M_rate,'*r'); 
grid on; 
xlabel('Pressure ratio'); 
ylabel('Mass Ratio'); 
%Plot Thermal efficiency vs Pressure ratio 
figure(2); 
plot(rp,N_thermal,'o-'); 
grid on; 
xlabel('Pressure Ratio'); 
ylabel('Thermal Efficiency'); 
%plot Temperature(K)vs Entropy(kJ/kg*k): 
%Table A-4 
TS=[273.01 0 9.1556;278.00 0.0763 9.0249;283.00 0.1511 8.8999; 
288.00 0.2245 8.7803;293.00 0.2965 8.6661;298.00 0.3672 8.5567; 
303.00 0.4368 8.4520;308.00 0.5051 8.3517;313.00 0.5724 8.2556; 
318.00 0.6386 8.1633;323.00 0.7038 8.0748;328.00 0.7680 7.9898; 
333.00 0.8313 7.9082;338.00 0.8937 7.8296;343.00 0.9551 7.7540; 
348.00 1.0158 7.6812;353.00 1.0756 7.6111;358.00 1.1346 7.5435; 
363.00 1.1929 7.4782;368.00 1.2504 7.4151;373.00 1.3072 7.3542; 
378.00 1.3634 7.2952;383.00 1.4188 7.2382;388.00 1.4737 7.1829; 
393.00 1.5279 7.1292;398.00 1.5816 7.0771;403.00 1.6346 7.0265; 
408.00 1.6872 6.9773;413.00 1.7392 6.9294;418.00 1.7908 6.8827; 
423.00 1.8418 6.8371;428.00 1.8924 6.7927;433.00 1.9426 6.7492; 
438.00 1.9923 6.7067;443.00 2.0417 6.6650;448.00 2.0906 6.6242; 
453.00 2.1392 6.5841;458.00 2.1875 6.5447;463.00 2.2355 6.5059; 
468.00 2.2831 6.4678;473.00 2.3305 6.4302;478.00 2.3776 6.3930; 
483.00 2.4245 6.3563;488.00 2.4712 6.3200;493.00 2.5176 6.2840; 
498.00 2.5639 6.2483;503.00 2.6100 6.2128;508.00 2.6560 6.1775; 
513.00 2.7018 6.1424;518.00 2.7476 6.1072;523.00 2.7933 6.0721; 
528.00 2.8390 6.0369;533.00 2.8847 6.0017;538.00 2.9304 5.9662; 
543.00 2.9762 5.9305;548.00 3.0221 5.8944;553.00 3.0681 5.8579; 
558.00 3.1144 5.8210;563.00 3.1608 5.7834;568.00 3.2076 5.7450; 
573.00 3.2548 5.7059;578.00 3.3024 5.6657;583.00 3.3506 5.6243; 
588.00 3.3994 5.5816;593.00 3.4491 5.5372;598.00 3.4998 5.4908; 
603.00 3.5516 5.4422;608.00 3.6050 5.3907;613.00 3.6602 5.3358; 
618.00 3.7179 5.2765;623.00 3.7788 5.2114;628.00 3.8442 5.1384; 
633.00 3.9165 5.0537;638.00 4.0004 4.9493;643.00 4.1119 4.8009; 
646.9500 4.4070 4.4070]; 
T_s=[318.81 0.6492;318.9952 0.6492;443.41 2.0457;444.4745 2.0457; 
536.94 2.9207;536.94 5.9737;623 6.4516;443.41 6.5759;318.81 6.9022; 
300 6.4516;650.85 6.5616;1100 10.32256;663.68 10.19586;420 8]; 
figure(3) 
plot(TS(:,2)',TS(:,1)','-m','LineWidth',1,'DisplayName','Saturation Line') 
hold on; grid on; 
plot(TS(:,3)',TS(:,1)','-m','LineWidth',1,'HandleVisibility','off') 
%plot Rankine/Steam cycle: 
for i=1:8  
plot(T_s(i:i+1,2),T_s(i:i+1,1),'-g','LineWidth',2,'HandleVisibility','off') 
end 
sat=[ 318.81 6.9022; 318.81 0.6492; 443.41 2.0457; 443.41 6.5759]; 
plot(sat(1:2,2),sat(1:2,1),'-g','LineWidth',1,'HandleVisibility','off') 
plot(sat(3:4,2),sat(3:4,1),'-g','LineWidth',1,'DisplayName','Steam Cycle') 
%plot Brayton/gas cycle: 
for i=10:12 
plot(T_s(i:i+1,2),T_s(i:i+1,1),'-r','LineWidth',1,'HandleVisibility','off') 
end 
plot(T_s(13:14,2),T_s(13:14,1),'-r','LineWidth',1,'DisplayName','Gas Cycle') 
plot(T_s(:,2),T_s(:,1),'.k') 
xlabel('Entropy(kJ/kg*k)'); 
ylabel('Temperature(K)'); 
axis([0 11 200 1600]); 
% Calculate mass ratio and thermal efficiency for each pressure ratio  
mass_ratio = M_rate';  
thermal_efficiency = N_thermal';  
% Create a table  
table_data = [rp' mass_ratio thermal_efficiency];  
table_headers = {'Pressure Ratio', 'Mass Ratio', 'Thermal Efficiency'}; 
table_results = array2table(table_data, 'VariableNames', table_headers);  
% Display the table 
disp('Table: Pressure Ratio, Mass Ratio, and Thermal Efficiency'); 
disp(table_results);
