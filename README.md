Procedure: 
clc; 
clear; 
% ---------------------- Input Parameters ---------------------- 
F = 100;                        
z = *0.5; 0.3; 0.2+;            
K = *2.0; 1.0; 0.5+;            
D = 40;                         
R = 1.5 * D;                    
B = F - D;                      
n = length(z);                  

 
% -------------------- Matrix Initialization -------------------- 
A = zeros(2*n, 3*n)            
b = zeros(2*n, 1)              
 
% ------------------ Equation Construction ------------------ 
for i = 1:n 
    % Equation 1: Overall component balance 
    % F*z = D*K*x1 + B*xB 
    A(i, i) = D * K(i);                   
    A(i, 2*n + i) = B;                 
    b(i) = F * z(i); 
 
    % Equation 2: Intermediate stage component balance 
    % F*z + R*x1 = B*xB + (R+D)*x2 
    A(n+i, i) = R;                       
    A(n+i, n + i) = -(R + D);             
    A(n+i, 2*n + i) = B;                 
    b(n+i) = -F * z(i); 
end 
 
% ------------------ Solve the System ------------------ 
x = A \ b;  
 
% ------------------ Extract and Normalize ------------------ 
x1 = x(1:n);              
x2 = x(n+1:2*n);            
xB = x(2*n+1:end);         
y1 = K .* x1;              
y2 = K .* x2;              
% Normalize to ensure mole fractions add to 1 
x1 = x1 / sum(x1); 
x2 = x2 / sum(x2); 
xB = xB / sum(xB); 
y1 = y1 / sum(y1); 
y2 = y2 / sum(y2); 
% ------------------ Display Output ------------------ 
fprintf('--- Multicomponent Partial Reflux Distillation Results ---\n\n'); 
for i = 1:n 
fprintf('Component %d:\n', i); 
fprintf('  x1 (rectifying)   = %.4f\n', x1(i)) 
fprintf('  y1 (top vapor)    = %.4f\n', y1(i)) 
fprintf('  x2 (stripping)    = %.4f\n', x2(i)) 
fprintf('  y2 (mid vapor)    = %.4f\n', y2(i)) 
fprintf('  xB (bottoms)      = %.4f\n\n', xB(i)) 
end 

