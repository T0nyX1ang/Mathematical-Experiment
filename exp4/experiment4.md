---
title: Experiment 4 Results
description: Experiment on numerical integrals.
layout: default
---

## Contents
* [Exercise 1](#exercise-1)
* [Exercise 2](#exercise-2)
* [Exercise 3](#exercise-3)
* [Exercise 4](#exercise-4)

## Exercise 1
```matlab
%% Exercise 1
% MatLab built-in integral
func1 = @(x) exp(-x.^2) ./ (1 + x.^4);
func2 = @(x) sin(x) ./ sqrt(1 - x.^2);
func3 = @(theta) (1 + cos(theta) + 1 + sin(theta));

result1 = integral(func1, -inf, +inf);
result2 = integral(func2, 0, 1);
result3 = integral(func3, 0, 2 * pi);

result1, result2, result3

% Monte-Carlo method
% question 1: make substitution x = arctan(t) to use finite method
value2 = monte_carlo(@(t) func1(atan(t)) ./ (1 + t.^2), 1, 1000000, -pi / 2, pi / 2)
value2 - result1

% question 1 method 2: using truncating function
value2 = monte_carlo(func1, 1, 1000000, -100, 100)
value2 - result1

% question 3: make polar substitution
value3 = monte_carlo(func3, 1, 1000000, 0, 2 * pi)
value3 - result3

function value = monte_carlo(funct, dimension, iteration, lb, ub)
    % monte-carlo main method
    points = zeros(dimension, iteration);
    total_value = 0;
    for i = 1: dimension
        points(i, :) = generate_random_points(iteration, lb(i), ub(i));
    end
    for j = 1: iteration
        total_value = total_value + funct(points(:, j)');
    end
    value = total_value * prod(ub - lb) / iteration;
end

function point = generate_random_points(numbers, lb, ub)
    % random point generator by uniform distribution
    point = lb + (ub - lb) .* rand(1, numbers);
end
```

## Exercise 2
```matlab
%% Exercise 2
gauss_legendre_coeff(2)
gauss_legendre_coeff(3)
gauss_legendre_coeff(4)

function gauss_legendre_coeff(total_vars)
    % constructing variables and symbols
    argument = sym('a', [1 total_vars]);
    variable = sym('x', [1 total_vars]);

    % constucting equations
    for i = 1: 2 * total_vars
        if (mod(i, 2) == 1)
            key = sym(2 / i);
        else
            key = sym(0);
        end
        equations(i) = sum(argument .* variable .^ (i - 1)) == key;
    end

    % solving equations
    sol = solve(equations, 'PrincipalValue', true);
    sol = struct2cell(sol);
    
    % displaying results
    disp("-- Gauss-Legendre coeffcient where order is " + total_vars + " --");
    for i = 1: total_vars
        disp("a" + int2str(i) + ": " + string(sol{i}) + ", x" + int2str(i) + ": " + string(sol{i + total_vars}));
    end
end
```

## Exercise 3
```matlab
%% Exercise 3
generic_integral_coeff(2, @(x) sqrt(x), 0, 1)
generic_integral_coeff(3, @(x) sqrt(x), 0, 1)
generic_integral_coeff(4, @(x) sqrt(x), 0, 1)

generic_integral_coeff(2, @(x) 1 ./ sqrt(1 - x .^ 2), -1, 1)
generic_integral_coeff(3, @(x) 1 ./ sqrt(1 - x .^ 2), -1, 1)
generic_integral_coeff(4, @(x) 1 ./ sqrt(1 - x .^ 2), -1, 1)

generic_integral_coeff(2, @(x) exp(-x), 0, +inf)
generic_integral_coeff(3, @(x) exp(-x), 0, +inf)
generic_integral_coeff(4, @(x) exp(-x), 0, +inf)

generic_integral_coeff(2, @(x) exp(-x .^ 2), -inf, +inf)
generic_integral_coeff(3, @(x) exp(-x .^ 2), -inf, +inf)
generic_integral_coeff(4, @(x) exp(-x .^ 2), -inf, +inf)

function generic_integral_coeff(total_vars, funct, left, right)
    if (nargin == 1)
        funct = @(x) 1;
    elseif (nargin == 2)
        left = -1;
        right = 1;
    end
    % constructing variables and symbols
    argument = sym('a', [1 total_vars]);
    variable = sym('x', [1 total_vars]);

    % constucting equations
    sym_funct = sym(funct);
    for i = 1: 2 * total_vars
        key = int(sym_funct .* sym(@(x) x .^ (i - 1)), left, right);
        equations(i) = sum(argument .* variable .^ (i - 1)) == key;
    end

    % solving equations
    sol = solve(equations, 'PrincipalValue', true);
    %sol = vpasolve(equations);
    sol = struct2cell(sol);
    
    % displaying results
    disp("-- Generic integral coeffcient where order is " + total_vars + " --");
    for i = 1: total_vars
        disp("a" + int2str(i) + ": " + string(sol{i}) + ", x" + int2str(i) + ": " + string(sol{i + total_vars}));
    end
end
```

## Exercise 4
```matlab
%% Exercise 4
% ginput test
[x1, y1] = ginput(); % first polynomial
plot([x1; x1(1)], [y1; y1(1)])
hold on;
[x2, y2] = ginput(); % second polynomial
plot([x2; x2(1)], [y2; y2(1)])
drawnow;
hold on;

square = calculate_square(x1, y1, x2, y2, 100000);
square

function final_square = calculate_square(x1, y1, x2, y2, total_points, epsilon)
    if (nargin == 5)
        epsilon = 1e-6;
    end

    x_min = min(min(x1), min(x2));
    x_max = max(max(x1), max(x2));
    y_min = min(min(y1), min(y2));
    y_max = max(max(y1), max(y2));

    total_square = (x_max - x_min) * (y_max - y_min);

    random_points = rand(total_points, 2);
    x_points = x_min * ones(total_points, 1) + (x_max - x_min) * random_points(:, 1);
    y_points = y_min * ones(total_points, 1) + (y_max - y_min) * random_points(:, 2);
    in_points = 0;

    valid = [];
    for i = 1: total_points
        if (interior([x_points(i) y_points(i)], x1, y1, epsilon) && (interior([x_points(i) y_points(i)], x2, y2, epsilon)))
            in_points = in_points + 1;
            valid = [valid; x_points(i), y_points(i)];
        end
    end

    scatter(valid(:, 1), valid(:, 2), 5, 'cyan');
    hold off;

    final_square = in_points / total_points * total_square;
end

function result = interior(point, xpoints, ypoints, epsilon)
    % judging a point is an interior of a polynomial
    if (nargin == 3)
        epsilon = 1e-6;
    end
    
    if (size(xpoints, 1) ~= size(ypoints, 1))
        error("Incompatible points");
    end
    
    number = size(xpoints, 1);
    angle_sum = 0;
    for i = 1: number
        angle_sum = angle_sum + get_angle(point, [xpoints(i) ypoints(i)], [xpoints(mod(i, number) + 1) ypoints(mod(i, number) + 1)]);
    end
    if (abs(abs(angle_sum) - 2 * pi) < epsilon)
        result = true;
    else
        result = false;
    end
    
end

function angle = get_angle(point1, point2, point3, epsilon)
    % calculate the angle through point 1 between point 2 and point 3
    if (nargin == 3)
        epsilon = 1e-6;
    end
    angle = acos((point2 - point1) * (point3 - point1)' / (norm(point2 - point1) * norm(point3 - point1)));
    validate_point3 = point1 + (point2 - point1) * [cos(angle) -sin(angle); sin(angle) cos(angle)] / norm(point2 - point1) * norm(point3 - point1);
    if (norm(validate_point3 - point3) >= epsilon)
        angle = -angle; % reverse rotation
    end
end
```
