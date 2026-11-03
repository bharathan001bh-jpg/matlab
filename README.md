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
A = zeros(2*n, 3*n)            % Coefficients matrix 
b = zeros(2*n, 1)             % Right-hand side vector 
 
% ------------------ Equation Construction ------------------ 
for i = 1:n 
    % Equation 1: Overall component balance 
    % F*z = D*K*x1 + B*xB 
    A(i, i) = D * K(i);                  % x1(i) 
    A(i, 2*n + i) = B;                   % xB(i) 
    b(i) = F * z(i); 
 
    % Equation 2: Intermediate stage component balance 
    % F*z + R*x1 = B*xB + (R+D)*x2 
    A(n+i, i) = R;                       % x1(i) 
    A(n+i, n + i) = -(R + D);            % x2(i) 
    A(n+i, 2*n + i) = B;                 % xB(i) 
    b(n+i) = -F * z(i); 
end 
 
% ------------------ Solve the System ------------------ 
x = A \ b;   % Solve Ax = b 
 
% ------------------ Extract and Normalize ------------------ 
x1 = x(1:n);               % Liquid in rectifying section 
x2 = x(n+1:2*n);           % Liquid in stripping section 
xB = x(2*n+1:end);         % Bottom product 
y1 = K .* x1;              % Vapor from rectifying section 
y2 = K .* x2;              
% Vapor from stripping section
