# I Plot You
A list of scripts that allows you to create awesome plots for your research. Annotations can be done automatically and the font of the labels and legend is compatible with ieee standards (no font 3).
The best way to use these notebooks is via google colab.

# Bar Plot with annotation

The notebook [bar_plot_with_annotation.ipynb](./bar_plot_with_annotation.ipynb) generates:

![img](./barplot.png)

## Details
Here you have 8 ``Patches`` objects, one per each drawed box. In order to use the annotation function you have to provide as argument to the function the initial and the final ``Patch``.
In this case, the bars we want to annotate have indexes 4 and 5 starting at 0 on the left most one:

``annotate_barplot_dataframe(4, 5, "*", patches, 1)``


# Horizontal Bar Plot with datapoints
The notebook [horizontal_bar_plot.ipynb](./horizontal_bar_plot.ipynb) generates:

![himg](./hbarplot.png)

## Details
Here, we use the library `seaborn` to overlay the datapoints to the horizontal bar plot.

with:
``
sns.swarmplot(x="score", y="type", data=new_df,
              size=5, color="1", linewidth=1, orient="h")
``
the datapoints will be stacked vertically, while with:

``
sns.stripplot(x="score", y="type", data=new_df,
              # size=5, color="1", linewidth=1)
``
the datapoints will be clustered.


# Scattered frequency
The notebook [scatter_frequency.ipynb](./scatter_frequency.ipynb) generates:

![scatter](./scatter.png)

Be aware that in the variable `s` should contain the occurences of the pair `x,y` in order to show appropriate results.

## Tips
Avoid the No Font 3 error from paperplaza with

```
import matplotlib as mtp
mtp.rcParams['pdf.fonttype'] = 42
```

## Credits
I have found the function for annotations, online some years ago. Is not mine but I adapted it to my needs.
If you know the author, would love to acknowledge! 


```python
def annotate_barplot_dataframe(bar0, bar1, text, patches, dh=0.2, c='black'):
    """annotate a grouped barplot from a pandas dataframe
    An annotation is built from bar0 to bar1

    Args:
        bar0 (int): index of first bar
        bar1 (int): index of second bar
        text (string): what to write on the annotation
        patches (matplotlib.patches): data source
    """
    patches.sort(key=lambda x: x.xy[0])
    left = patches[bar0]
    right = patches[bar1]
    y = max(left._height, right._height) + dh

    l_bbox = left.get_bbox()
    l_mid = l_bbox.x1 - left._width / 2

    r_bbox = right.get_bbox()
    r_mid = r_bbox.x1 - right._width / 2
    barh = 0.07
    barx = [l_mid, l_mid, r_mid, r_mid]
    bary = [
        y,
        y + barh,
        y + barh,
        y,
    ]  # lower-left, upper-left, upper-right, lower-right
    plt.plot(barx, bary, c=c)
    kwargs = dict(ha="center", va="bottom")
    # if fs is not None:
    # kwargs['fontsize'] = fs
    mid = ((l_mid + r_mid) / 2, y + 0.01)
    plt.text(*mid, text, **kwargs)
```