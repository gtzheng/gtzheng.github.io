---
layout: post
title: Plotting in Python
---
Import packages for plotting
```python
import matplotlib.pyplot as plt
import matplotlib as mpl
from PIL import Image
from mpl_toolkits.axes_grid1 import ImageGrid
plt.rcParams["savefig.bbox"] = 'tight'
```
## Basics

First, create a figure using `subplots` function

```python
fig, ax = plt.subplots(nrows=1, ncols=1, figsize=(10,6))
```
Of course, we can use `plt.plot` to directly plot a figure, but you have less control over the figure you create. 
For example, in the above command, you can specify the figure size, how many subfigures you need, etc..
So I choose this as my starting point of creating a figure. 

Calling `subplots()` without specifying any parameters will create one figure with the default size.
Setting `figsize=(10,6)` will create a figure with width 10 and height 6.
Setting `nrows` and `ncols` will create a figure with `nrows` X `ncols` axes, i.e., `ax` will be a `nrow`-by-`ncols` array.

In the following, I list several commonly used functions in Python (specifically, matplotlib):
- Basic plotting functions: `ax.plot`, `ax.scatter`, `ax.bar`, etc.
- Set x(y)-axis label: `ax.set_xlabel`, `ax.set_ylabel`
- Set title label: `ax.title`
- Set x(y)-tick labels: `ax.set_xticklabels([])`, `ax.set_yticklabels([])`. If the argument is an empty list, then no tick labels will be shown
- Set x(y)-tick label locations: `ax.get_xaxis().set_ticks([])`, `ax.set_yticklabels([])`. If the argument is an empty list, then the small vertical lines indicating the 
tick label locations will not be shown. The length of a tick label list and the length of a tick location list must match.
- Set x(y) limits: `ax.set_xlim`, `ax.set_ylim`
- Set label size: `plt.rcParams['axes.labelsize']`
- Set title size: `plt.rcParams['axes.titlesize']`
- Set x(y)-tick label size: `plt.rcParams['xtick.labelsize']`, `plt.rcParams['ytick.labelsize']`

If `ax` is a matrix, we can index the Axes object on the $i$-th row and $j$-th column as `ax[i,j]`, and we can use `ax[i,j].plot` and other functions listed above.

## Choose Colors from Color Maps
Sometimes we need to assign different colors to different lines or points. We can manually choose the set of colors but it is tedious if you need many colors. 
An easier way is to choose colors from a color map using the following code:

```
cmap = plt.get_cmap(cmap_name)
colors = cmap(np.linspace(0, 1, 5))
```
`cmap_name` could be 'viridis', 'plasma', 'inferno', or other names in [cmaps](https://matplotlib.org/3.5.0/tutorials/colors/colormaps.html).
Consider each color in a color map as a point in the interval $[0,1]$, then we can choose a color from a chosen color map by giving a real number in the range $[0,1]$.
Here, I choose 5 colors with equal distance in the chosen color map. Each row of `colors` represent a color. 

## Add a Colorbar

```
cnorm = mpl.colors.Normalize(vmin=min_val, vmax=max_val, clip=False)   
cmapper = mpl.cm.ScalarMappable(norm=cnorm, cmap='RdYlBu')
cb = fig.colorbar(cmapper, ax=ax)
cb.ax.tick_params(labelsize=18)
```
The first two lines define a color mapper which converts values in [min_val, max_val] to the corresponding colors in a chosen color map.
Then, the color mapper `cmapper` is used to initialize a colorbar which is attached to `ax` in `fig`. The last line changes the fontsize of the text in the colorbar.
Note that `fig` and `ax` are obtained via `plt.subplots()` as shown in the very beginning.

## Create an Image Grid
Use `ImageGrid` to create a grid of Axes objects, and you can show your images by calling the plotting functions of each axes object.
In the example code below, I use `ImageGrid` to create a `grid` object which contains $4\times 7$ Axes objects. Next, I put the image data 
into `image_arr` which is a list of 28 images. When iterating through `image_arr` and `grid`,  images are first plotted from left to right,
Once a row is filled with images, then the remaining images are plotted in the next row and from left to right. Most of lines inside the for loop
are used to remove the border, grid, ticks, and tick labels of a plotted image.

```
fig = plt.figure(figsize=(9., 6.))
grid = ImageGrid(fig, 111,  # similar to subplot(111)
                 nrows_ncols=(4, 7), 
                 axes_pad=0.05,  # pad between axes in inch.
                 )
for i, (ax, im) in enumerate(zip(grid, image_arr)):
    ax.imshow(im)
    ax.grid(False)
    ax.spines['top'].set_visible(False)
    ax.spines['right'].set_visible(False)
    ax.spines['bottom'].set_visible(False)
    ax.spines['left'].set_visible(False)
    ax.set_yticklabels([])
    ax.set_xticklabels([])
    ax.get_xaxis().set_ticks([])
    ax.get_yaxis().set_ticks([])
```
