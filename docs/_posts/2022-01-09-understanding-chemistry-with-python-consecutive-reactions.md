---
title: "Understanding chemistry through Python: Consecutive Reactions"
categories:
  - Tutorial
  - Python
  - Chemistry
---

# Understanding chemistry through Python: Consecutive Reactions
Chemists are, unlike physicists or mathematicians, not often related to proficient programming skills. During our first bachelors degree, my fellow students and I were briefly introduced to programming in Python. However, due to the lack of real-world problems, most of us failed to see the true power of Python for scientific computing, automation and data analysis. Many of us, including me, condemned Python to be nothing more than a vague nightmare during our curriculum (yes, the programming course was really hard).

Two years passed before we touched Python again. Our professor in physical chemistry impulsively challenged us to write a program to visualize/simulate a consecutive reaction. Reward for the first 5 succesful implementations: 10% increase on our final course grade!
This made gaining insights in consecutive reactions the first 'real-world' problems that I've tackled with Python. It was also the moment where I started to appreciate Python, up to the point where it is now my most-used (if not only) tool for my research.

It is therefore that I write this small tutorial on a sunday evening with great joy, and hope that I can aspire some other chemists (or scientists in general) to pick up Python programming for their research!

## Theory of consecutive reaction kinetics
Chemical reactions can be roughly separated in elementary reactions and complex reactions. The latter can be understood as a combination of multiple elementary reactions, where an elementary reaction forms a reaction intermediate that serves as a reactant for another chemical reaction. The combination of those steps is then called a reaction mechanism. Unveiling reaction mechanisms is a major focus for many chemists!

In our simple example, we will consider a simple, irreversible consecutive reaction of a component <!-- $A$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=A"> that reacts to <!-- $C$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=C"> through intermediate <!-- $B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=B">:
<!-- $A \xrightarrow{k_1} B \xrightarrow{k_2} C$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=A%20%5Cxrightarrow%7Bk_1%7D%20B%20%5Cxrightarrow%7Bk_2%7D%20C">

### Reaction rate
The rate with which a reactant converts to a product is called a reaction rate. This can however only be conveniently build for elemental reactions. But since we learned that complex reactions are, in the end, a combination of elemental steps with intermediate products, we can thus expect that the reaction rate from a complex reaction is a mathematical combination of elementary reaction rates.

The reaction rate of an elementary reaction depends on the reactant concentration <!-- $[Reactant]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BReactant%5D"> and rate constant <!-- $k$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=k">.
For the first reaction <!-- $A \xrightarrow{k_1} B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=A%20%5Cxrightarrow%7Bk_1%7D%20B">, the reaction rate is:
<!-- $\frac{d[A]}{dt}=-k_1[A]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5Cfrac%7Bd%5BA%5D%7D%7Bdt%7D%3D-k_1%5BA%5D">.
This equation shows the change in <!-- $[A]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BA%5D"> over an infinitely small time interval <!-- $t$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=t">. <!-- $-k_1$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=-k_1"> has units <!-- $s^{-1}$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=s%5E%7B-1%7D">, such that the unit of our expression will become <!-- $\frac{mol}{s}$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5Cfrac%7Bmol%7D%7Bs%7D">. To have an expression for <!-- $[A]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BA%5D"> after a certain time <!-- $t$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=t">, we should integrate the expression over our time interval:
<!-- $[A]=[A]_0 e^{-k_1t}$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BA%5D%3D%5BA%5D_0%20e%5E%7B-k_1t%7D">

To calculate <!-- $[B]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BB%5D">, we should look at how fast <!-- $B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=B"> is being formed but also to how fast $B$ reacts to <!-- $C$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=C">:
<!-- $\frac{d[B]}{dt}=k_1[A]-k_2[C]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5Cfrac%7Bd%5BB%5D%7D%7Bdt%7D%3Dk_1%5BA%5D-k_2%5BC%5D">
Which we can again convert:
<!-- $[B]=\frac{k_1[A]_0}{k_2-k_1} \left( k_2e^{-k_1t}-k_1e^{-k_2t} \right)$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BB%5D%3D%5Cfrac%7Bk_1%5BA%5D_0%7D%7Bk_2-k_1%7D%20%5Cleft(%20k_2e%5E%7B-k_1t%7D-k_1e%5E%7B-k_2t%7D%20%5Cright)">

The last unknown, <!-- $[C]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BC%5D">, is expressed again in a similar way as the conversion from <!-- $[B]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BB%5D"> with rate constant <!-- $k_2$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=k_2">:
<!-- $\frac{d[C]}{dt}=k_2[B]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5Cfrac%7Bd%5BC%5D%7D%7Bdt%7D%3Dk_2%5BB%5D">

By using the previous equations, one can eventually find:
<!-- $[C]=[A]_0 \left(1+\frac{1}{k_1-k_2}\left( k_2e^{-k_1t}-k_1e^{-k_2t}\right) \right)$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BC%5D%3D%5BA%5D_0%20%5Cleft(1%2B%5Cfrac%7B1%7D%7Bk_1-k_2%7D%5Cleft(%20k_2e%5E%7B-k_1t%7D-k_1e%5E%7B-k_2t%7D%5Cright)%20%5Cright)">

It is interesting that, given the rate constant for the reactions, all concentrations at time <!-- $t$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=t"> can be calculated from the initial concentration of reactant <!-- $[A]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BA%5D">. Now let's go the fun part and implement this in Python!

--------------------------------------------------------------

## Implementation in Python
### Installation of Dependencies
I assume that you have already installed python on your system. If not, there are plenty of tutorials available on how to do so for your operating system. Further, we rely on 2 modules: `numpy` and `matplotlib`. `numpy` is a linear algebra library that allows us to use arrays to represent vectors and matrices. Apart from the mathematical operations that it introduces, it is blazing fast compared with standard Python code! `Matplotlib` on the other hand is a widely used library to plot high-quality, publication-ready graphs and figures of your data.

You should check if you have installed these by importing them in a Python shell:
```python
$ python

>>> import numpy as np
>>> import matplotlib.pyplot as plt
```
When running those lines, you should just be prompted a new line without any output. If you receive an error in one of those lines, you most likely haven't installed them.
you can conveniently install these through `pip` by running (in a Bash shell!):
```
$ pip install [package]
```

To install matplotlib and numpy, you should therefore run:
```
$ pip install numpy
$ pip install matplotlib
```

Now retry to import them in a Python shell.

### Calculating the concentrations
The time and concentration data will be represented as numpy vectors. Therefore, we should first import the numpy library by running `import numpy as np`.
Now we can proceed to implement the derived equations in Python. First of all, we should define the initial concentration of <!-- $[A]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BA%5D">, the time interval of interest and the rate constants of the two elemental reactions:
```python
time = np.arange(0.0, 5.0, 0.001)
conc_A_0 = float(10)
k1, k2 = 1, 2
```
Here we created a numpy vector `time` that represents our time interval. It starts at `t=0.0` seconds up until `t=4.999` seconds with intervals of 1 millisecond. Subsequently, we set the initial concentration of <!-- $[A]_0$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BA%5D_0"> to 10 <!-- $\frac{mol}{L}$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5Cfrac%7Bmol%7D%7BL%7D">. Finally, we set the rate constant for reactions to 1 and 2. Feel free to play around with those values or implement an interactive way to change them! (hint: look at [matplotlib sliders](https://matplotlib.org/stable/gallery/widgets/slider_demo.html))

For <!-- $[A]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BA%5D">, the equation is fairly simple:
```python
conc_A = conc_A_0 * np.exp(-k1*time)
```

The equations of <!-- $[B]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BB%5D"> and <!-- $[C]$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BC%5D"> are a bit more complicated, but rather straightforward nonetheless:
```python
conc_B = conc_A_0 * (k1/(k2-k1))*(np.exp(-k1*time)-np.exp(-k2*time))
conc_C = conc_A_0 * (1 + (1/(k2-k1))*(k1 * np.exp(-k2 * time) - k2 * np.exp(-k1 * time)))
```

### Plotting the data
To plot the data, we rely on matplotlib. It is by far the most used plotting and graphing library available for Python. You can import it by running `import matplotlib.pyplot as plt`.

Now we can create a `figure()` and `axes` object by running:
```python
fig, ax = plt.subplots()
```

Plotting the 3 concentrations in function of time requires the following lines of code:
```python
ax.plot(conc_a, time, label='concentration A')
ax.plot(conc_b, time, label='concentration B')
ax.plot(conc_c,	time, label='concentration C')
```

Now making the graph a bit nicer by setting labels and including a legend:
```python
ax.ylabel('concentration (mol/l)')
ax.xlabel('time (s)')
ax.legend(True)
```

And our plot is ready! You can view it by running `plt.show()` after running all the previous lines.


![cons_reaction.png](/assets/post_images/cons_reaction.png)


As we can see, the concentration of <!-- $A$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=A"> starts at <!-- $[A]_0$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5BA%5D_0"> but immediately converts irreversibly to <!-- $B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=B">, causing a bumb in concentration of <!-- $B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=B">. But remember that <!-- $B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=B"> is the fuel for the second elemental reaction: <!-- $C$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=C"> is being formed! Now that the amount of <!-- $A$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=A"> in the system is drastically decreasing, the formation of <!-- $B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=B"> slows down. The conversion rate of <!-- $B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=B"> to <!-- $C$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=C"> still increases, up until the conversion to <!-- $C$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=C"> is faster than the formation of <!-- $B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=B">: we have reached our maximum <!-- $B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=B"> concentration in our time interval. Now it is just a matter of time before everything is converted to <!-- $C$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=C">.

### Total Script
```python
# import the libraries
import numpy as np  # numerical library
import matplotlib.pyplot as plt  # graph-plottig library

# Set up the initial information
k1 = 1
k2 = 2
conc_A_0 = float(10)
time = np.arange(0.0, 5.0, 0.001)

# Calculate the concentrations of the 3 components
conc_A = conc_A_0 * np.exp(-k1*time)
conc_B = conc_A_0 * (k1/(k2-k1))*(np.exp(-k1*time)-np.exp(-k2*time))
conc_C = conc_A_0 * (1 + (1/(k2-k1))*(k1 * np.exp(-k2 * time) - k2 * np.exp(-k1 * time)))

# plot the data
fig, ax = plt.subplots()

ax.plot(time, conc_A, label='concentration A')
ax.plot(time, conc_B, label='concentration B')
ax.plot(time, conc_C, label='concentration C')

ax.set_ylabel('concentration (mol/l)')
ax.set_xlabel('time (s)')
ax.legend()

plt.show()

```

## Closing Remarks
We've succesfully modeled a simple, fictive consecutive reaction! You could now search for other complex reactions with multiple intermediates and try to model those as well. 

I hope you liked this basic tutorial, and feel free to give feedback and suggestions or request a particular tutorial.
