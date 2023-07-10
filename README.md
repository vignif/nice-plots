# I Plot You
A list of scripts that allows you to create awesome plots for your research. Annotations can be done automatically and the font of the labels and legend is compatible with ieee standards (no font 3)

# Bar Plot with annotation
![img](./barplot.png)

# Horizontal Bar Plot with annotation
![himg](./hbarplot.png)

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