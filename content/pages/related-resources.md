---
content_type: page
description: This section includes links to resources related to this course.
draft: false
learning_resource_types: []
ocw_type: CourseSection
title: Related Resources
uid: 62c2fe9d-a7d9-e5de-8e33-cd8322c52851
---
This page contains engine performance specifications and scripts for determining engine exhaust composition.

{{% resource_link c9ded809-347d-94a8-0175-53bb4385e84c "Engine Performance Specifications (XLSX)" %}}

### Main program to call exhaust\_compo and plots results: exhaust\_compo\_main.m 

```matlab
%main driver for exhaust_compo
%Gasoline CHmOn;
m=1.87;n=0;
Lambda=[0.6:0.1:1.4];
[xco2,xh2o,xco,xh2,xo2,xn2]=exhaust_compo(m,n,Lambda);
h=plot(Lambda,[xco2,xh2o,xco,xh2,xo2],'-');
xlabel('\lambda');
ylabel('Exhaust Mole fractions');
legend('CO2','H2O','CO','H2','O2');
```

### Function subroutine to calculate the composition: exhaust\_compo.m 

```matlab
function [xco2,xh2o,xco,xh2,xo2,xn2]=exhaust_compo(m,n,Lambda); 
%calculate xhaust mole fractions 
%For lean to stoichiometric combustion, assume complete burning 
%For rich combustion, assume water gas equilibrium constant k=3.7 
%Input: m,n are the hydrogen and oxygen to carbon ratios of the fuel 
%Input: Lambda is the column vector of air equivalence ratio 
%Output are the mole fractions of the exhaust composition 
k=3.7; 
nL=length(Lambda); 
xco2=zeros(nL,1); 
xco=zeros(nL,1); 
xh2o=zeros(nL,1); 
xh2=zeros(nL,1); 
xo2=zeros(nL,1); 
xn2=zeros(nL,1);

for i=1:nL; 
L=Lambda(i); 
if (L<=0);error('Lambda must be positive');end; if(L>=1);%=============== Lambda >=1 ================== 
z=1+0.25m-0.5n; 
Lm1=L-1; 
co=0;h2=0;co2=1;h2o=0.5m; n2=z3.773L; o2=Lm1z; 
else; %=============== Lambda < 1 ================== 
z=n(1-L)+2L(1+0.25m); 
B=2+(0.5m-1)k-(1-k)z; A=0.5m(1-k); C=k(1-z); 
b=(-B+sqrt(B.B-4AC))/(2A); 
a=b./(k+(1-k)b); co2=a; co=1-a; h2o=0.5mb; h2=0.5m(1-b); n2=(1+0.25m-0.5n)3.773*L; 
o2=0; 
end %============================================= 
total=co+h2+co2+h2o+n2+o2; 
xco(i)=co./total; 
xh2(i)=h2./total; 
xco2(i)=co2./total; 
xh2o(i)=h2o./total; 
xo2(i)=o2./total; 
xn2(i)=n2./total; 
end
```