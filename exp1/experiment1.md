---
title: Experiment 1 Results
description: Experiment on data visualizations.
layout: default
---

<head>
      <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
   <!--
This HTML was auto-generated from MATLAB code.
To make changes, update the MATLAB code and republish this document.
      --><meta name="generator" content="MATLAB 9.4"><link rel="schema.DC" href="http://purl.org/dc/elements/1.1/"><meta name="DC.date" content="2019-03-09"><meta name="DC.source" content="experiment1.m"><style type="text/css">

p { padding:0px; margin:0px 0px 20px; }
img { padding:0px; margin:0px 0px 20px; border:none; }
p img, pre img, tt img, li img, h1 img, h2 img { margin-bottom:0px; } 

pre, code { font-size:12px; }
tt { font-size: 1.2em; }
pre { margin:0px 0px 20px; }
pre.codeinput { padding:10px; border:1px solid #d3d3d3; background:#f7f7f7; }
pre.codeoutput { padding:10px 11px; margin:0px 0px 20px; color:#4c4c4c; }
pre.error { color:red; }

@media print { pre.codeinput, pre.codeoutput { word-wrap:break-word; width:100%; } }

span.keyword { color:#0000FF }
span.comment { color:#228B22 }
span.string { color:#A020F0 }
span.untermstring { color:#B20000 }
span.syscmd { color:#B28C00 }

  </style></head><body><div class="content"><h2>Contents</h2><div><ul><li><a href="#1">Exercise 1</a></li><li><a href="#2">Exercise 2</a></li><li><a href="#3">Exercise 3</a></li><li><a href="#4">Exercise 4</a></li><li><a href="#5">Exercise 5</a></li><li><a href="#6">Exercise 6</a></li><li><a href="#7">Exercise 7</a></li><li><a href="#8">Exercise 8</a></li><li><a href="#9">Exercise 9</a></li></ul></div><h2 id="1">Exercise 1</h2><pre class="codeinput">clear, clc;
x = linspace(-2, 2, 1000);
y1 = x.^2;
y2 = sin(x);
p1 = plot(x, y1);
hold <span class="string">on</span>;
p2 = plot(x, y2);
hold <span class="string">off</span>;
legend([p1, p2], <span class="string">'y = x^2'</span>, <span class="string">'y = sin(x)'</span>);
</pre><img vspace="5" hspace="5" src="experiment1_01.png" alt=""> <h2 id="2">Exercise 2</h2><pre class="codeinput">clear, clc;
x = linspace(0, 4 * pi, 1000);
g = @(t) (1 - t.^2).^(3 / 2);
y = arrayfun(@(x) integral(g, 0, sin(x)), x);
plot(x, y);
</pre><img vspace="5" hspace="5" src="experiment1_02.png" alt=""> <h2 id="3">Exercise 3</h2><pre class="codeinput">clear, clc;
x = linspace(-4, 4, 50);
y = linspace(-4, 4, 50);
[xx, yy] = meshgrid(x, y);
zz = sin(xx .* yy) .* (xx.^2) .* (yy.^3);
figure(1);
mesh(xx, yy, zz);
title(<span class="string">'Mesh plot'</span>);
figure(2);
contour(xx, yy, zz, 15);
title(<span class="string">'Contour plot'</span>);
figure(3);
pcolor(zz);
colorbar;
colormap(cool);
title(<span class="string">'Pesudo color plot'</span>);
figure(4);
surf(xx, yy, zz);
title(<span class="string">'Surface plot'</span>);
</pre><img vspace="5" hspace="5" src="experiment1_03.png" alt=""> <img vspace="5" hspace="5" src="experiment1_04.png" alt=""> <img vspace="5" hspace="5" src="experiment1_05.png" alt=""> <img vspace="5" hspace="5" src="experiment1_06.png" alt=""> <h2 id="4">Exercise 4</h2><pre class="codeinput">clear, clc;
x = linspace(-1, 1, 50);
y = linspace(-2, 2, 50);
t = linspace(-3, 3, 50);
[xx, yy, tt] = meshgrid(x, y, t);
v = (xx.^2) .* (yy.^3) .* (tt.^4);
xi = [-0.5, -0.2, 0.2, 0.5];
yi = [-0.7 0.3 1.2];
ti = [-2.5, -1.5, 0.8, 2.4];
slice(xx, yy, tt, v, xi, yi, ti);
xlabel(<span class="string">'x'</span>);
ylabel(<span class="string">'y'</span>);
zlabel(<span class="string">'z'</span>);
colorbar(<span class="string">'horiz'</span>);
title(<span class="string">'Slice plot'</span>);
</pre><img vspace="5" hspace="5" src="experiment1_07.png" alt=""> <h2 id="5">Exercise 5</h2><pre class="codeinput">clear, clc;
x = linspace(-2 * pi, 2 * pi, 50);
y = linspace(-2 * pi, 2 * pi, 50);
t = linspace(0, 10, 1000);
[xx, yy] = meshgrid(x, y);
<span class="keyword">for</span> i = 1: size(t, 2)
    zz = 10 .* exp(1 - t(i)) .* sin(xx) .* cos(yy);
    surf(xx, yy, zz)
    title(<span class="string">"Plot where t = "</span> + num2str(t(i)));
    drawnow;
<span class="keyword">end</span>
</pre><img vspace="5" hspace="5" src="experiment1_08.png" alt=""> <h2 id="6">Exercise 6</h2><pre class="codeinput">clear, clc;
x = linspace(-2, 2, 20);
y = linspace(-2, 2, 20);
[xx, yy] = meshgrid(x, y);
zz = xx .* yy;
[px, py] = gradient(zz, .1, .1);
contour(xx, yy, zz);
hold <span class="string">on</span>;
quiver(xx, yy, px, py);
hold <span class="string">off</span>;
axis <span class="string">image</span>;
</pre><img vspace="5" hspace="5" src="experiment1_09.png" alt=""> <h2 id="7">Exercise 7</h2><pre class="codeinput">clear, clc;
total = 2000;
x = 1;
y = 1;
rec = [0.5 0 0 0.5 0 0; 0.5 0 0 0.5 0.5 0; 0.5 0 0 0.5 0.25 0.5];
<span class="keyword">for</span> i = 1: total
    plot(x, y, <span class="string">'.'</span>);
    hold <span class="string">on</span>;
    drawnow;
    select = randi(3);
    ox = x;
    oy = y;
    x = rec(select, 1) * ox + rec(select, 3) * oy + rec(select, 5);
    y = rec(select, 2) * ox + rec(select, 4) * oy + rec(select, 6);
<span class="keyword">end</span>
hold <span class="string">off</span>;
</pre><img vspace="5" hspace="5" src="experiment1_10.png" alt=""> <h2 id="8">Exercise 8</h2><pre class="codeinput">clear, clc;
fimplicit(@(x, y) sin(x .* y) + x + y, [-5 5 -5 5]);
</pre><img vspace="5" hspace="5" src="experiment1_11.png" alt=""> <h2 id="9">Exercise 9</h2><pre class="codeinput">clear, clc;
datadata = load(<span class="string">'./data.mat'</span>);
data = datadata.xx;
data(isnan(data(:, 1)), :) = [];
data(isnan(data(:, 2)), :) = [];
scatter(data(:, 1), data(:, 2));
</pre><img vspace="5" hspace="5" src="experiment1_12.png" alt=""> <p class="footer"><br><a href="https://www.mathworks.com/products/matlab/">Published with MATLAB&reg; R2018a</a><br></p></div><!--
##### SOURCE BEGIN #####
%% Exercise 1
clear, clc;
x = linspace(-2, 2, 1000);
y1 = x.^2;
y2 = sin(x);
p1 = plot(x, y1);
hold on;
p2 = plot(x, y2);
hold off;
legend([p1, p2], 'y = x^2', 'y = sin(x)');

%% Exercise 2
clear, clc;
x = linspace(0, 4 * pi, 1000);
g = @(t) (1 - t.^2).^(3 / 2);
y = arrayfun(@(x) integral(g, 0, sin(x)), x);
plot(x, y);

%% Exercise 3
clear, clc;
x = linspace(-4, 4, 50);
y = linspace(-4, 4, 50);
[xx, yy] = meshgrid(x, y);
zz = sin(xx .* yy) .* (xx.^2) .* (yy.^3);
figure(1);
mesh(xx, yy, zz);
title('Mesh plot');
figure(2);
contour(xx, yy, zz, 15); 
title('Contour plot');
figure(3);
pcolor(zz);
colorbar;
colormap(cool);
title('Pesudo color plot');
figure(4);
surf(xx, yy, zz);
title('Surface plot');

%% Exercise 4
clear, clc;
x = linspace(-1, 1, 50);
y = linspace(-2, 2, 50);
t = linspace(-3, 3, 50);
[xx, yy, tt] = meshgrid(x, y, t);
v = (xx.^2) .* (yy.^3) .* (tt.^4);
xi = [-0.5, -0.2, 0.2, 0.5];
yi = [-0.7 0.3 1.2];
ti = [-2.5, -1.5, 0.8, 2.4];
slice(xx, yy, tt, v, xi, yi, ti);
xlabel('x');
ylabel('y');
zlabel('z');
colorbar('horiz');
title('Slice plot');

%% Exercise 5
clear, clc;
x = linspace(-2 * pi, 2 * pi, 50);
y = linspace(-2 * pi, 2 * pi, 50);
t = linspace(0, 10, 1000);
[xx, yy] = meshgrid(x, y);
for i = 1: size(t, 2)
    zz = 10 .* exp(1 - t(i)) .* sin(xx) .* cos(yy);
    surf(xx, yy, zz)
    title("Plot where t = " + num2str(t(i)));
    drawnow;
end

%% Exercise 6
clear, clc;
x = linspace(-2, 2, 20);
y = linspace(-2, 2, 20);
[xx, yy] = meshgrid(x, y);
zz = xx .* yy;
[px, py] = gradient(zz, .1, .1);
contour(xx, yy, zz);
hold on;
quiver(xx, yy, px, py);
hold off;
axis image;

%% Exercise 7
clear, clc;
total = 2000;
x = 1;
y = 1;
rec = [0.5 0 0 0.5 0 0; 0.5 0 0 0.5 0.5 0; 0.5 0 0 0.5 0.25 0.5];
for i = 1: total
    plot(x, y, '.');
    hold on;
    drawnow;
    select = randi(3);
    ox = x;
    oy = y;
    x = rec(select, 1) * ox + rec(select, 3) * oy + rec(select, 5);
    y = rec(select, 2) * ox + rec(select, 4) * oy + rec(select, 6);
end
hold off;

%% Exercise 8
clear, clc;
fimplicit(@(x, y) sin(x .* y) + x + y, [-5 5 -5 5]);

%% Exercise 9
clear, clc;
datadata = load('./data.mat');
data = datadata.xx;
data(isnan(data(:, 1)), :) = [];
data(isnan(data(:, 2)), :) = [];
scatter(data(:, 1), data(:, 2));
##### SOURCE END #####
--></body>