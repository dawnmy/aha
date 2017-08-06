---
title: "Boxplot with breaks on y-axis using Python"
date: 2017-08-06T21:59:26+02:00
draft: false
---

> Boxplot is extremely useful to illustrate the distribution of your data statistically. However, in some cases there are massive difference on distribution between the boxes. And we do not want to show the whole y-axis as this will make the data points unvisible especially when the figure size is limited. Here is a example showing how to depict such data in a decent way with breaks on y-aixs using `matplotlib`. `matplotlib` does not have a build-in functionality to easily  make such plot like `Origin`. Therefore I accomplished this need in a bit complex alternative way.

```python
import seaborn as sns
import matplotlib.pyplot as plt
f, (ax, ax2) = plt.subplots(2, 1, sharex=True)
plt.rcParams['svg.fonttype'] = 'none'
plt.rcParams['pdf.fonttype'] = 42
sns.set(font="Arial", style="whitegrid", context='paper', font_scale=2)

# plt.style.use('fivethirtyeight')
sns.boxplot(data=df5, x='Virus', y='percentage', hue='state', ax=ax, palette='husl', linewidth=0.5, fliersize=3.7)
sns.boxplot(data=df5, x='Virus', y='percentage', hue='state', ax=ax2, palette='husl', linewidth=0.5, fliersize=3.7)
ax.set_ylim(70, 100)  # outliers only
ax2.set_ylim(0, 10)  # most of the data points
ax.spines['bottom'].set_visible(False)
ax.get_xaxis().set_visible(False)

ax2.spines['top'].set_visible(False)
ax.xaxis.tick_top()
ax.tick_params(labeltop='off')  # no tick labels at the top

ax2.xaxis.tick_bottom()

d = .015  # size of diagonal lines on axis

kwargs = dict(transform=ax.transAxes, color='k', clip_on=False)
ax.plot((-d, +d), (-d, +d), **kwargs)        # top-left diagonal
ax.plot((1 - d, 1 + d), (-d, +d), **kwargs)  # top-right diagonal

kwargs.update(transform=ax2.transAxes)  # switch to the bottom axis
ax2.plot((-d, +d), (1 - d, 1 + d), **kwargs)  # bottom-left diagonal
ax2.plot((1 - d, 1 + d), (1 - d, 1 + d), **kwargs)  # bottom-right diagonal

plt.xticks(rotation=60)

```
The figure needs some adjustment with vector graph editor as `AI`.


