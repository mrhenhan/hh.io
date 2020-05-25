---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Time as a Machine Learning Feature"
subtitle: "A method to represent cyclic continuous data"
summary: "Quite often it is the case that cyclic data is not sufficiently transformed for machine learning algorithms, e.g. feature representation is missing out on the implicit properties of cyclic features often resulting in wrong distance measures. This article introduces cyclic feature transformation for time based features as a mini-howto."
authors: ["mrhenhan"]
tags: ["time", "cyclic features", "machine learning", "feature representation", "time encoding"]
categories: ["machine learning", "feature representation", "data encoding", "mini-howto"]
date: 2020-05-20T15:07:30+02:00
lastmod: 2020-05-20T15:07:30+02:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
There are many kinds of data naturally following cycles, like longitude, rotation, positions and movements of atronomical objects or tides for example. The way we measure time is one of the upiquitous primary application areas of cyclic data.

_But is the way we use to handle and display time values also suitable for expressing and introducing cyclicality to a machine learning model?_

To make it quick $\rightarrow$ **No**.

The reason for this is, that "cyclicity" is only an implicit feature of time known to us, as we have learned to cope with it in our childhood, which means we are able to understand distances between two adjacent points in time on the cyclic scale implicitely. A machine learning algorithm using a regularily scaled time feature, e.g. represented as 24h time, will interpret some distances between two adjacent points in time wronlgy.

On a 24h clock, the difference between 23:00 and 03:00 is 4h but the difference between 03:00 and 23:00 is 20h. In order to prevent bias of this kind and to introduce the cyclic behaviour to our machine learning models, a time representation in the form of sinus-cosinus value pairs has become established.

The following python code shows how time can be encoded in form of sinus-cosinus value pairs.

At first we generate some random points in time and import some default libraries and do something about the plotting style. The import looks like much, but only _numpy_, _pandas_ and _matplotlib.pyplot_ are really needed.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

colors = ["windows blue", "amber", "greyish", "faded green", "dusty purple"]
sns.palplot(sns.xkcd_palette(colors))

plt.rcParams["figure.figsize"] = (20,10)
sns.set()

SECONDS_IN_DAY = 24 * 60 * 60
N_TIMES = 2500

def gimme_times(n):
    rnd_sec = np.random.randint(0, SECONDS_IN_DAY, n)
    return pd.Series(rnd_sec, name="Seconds").sort_values().reset_index(drop=True)
    

times = gimme_times(N_TIMES)
times.head()
```

The former code will produce an output similar to the following table.

I   | Seconds
--- | --------
0   | 13
1   | 25
2   | 34
3   | 64
4   | 71
**Name: Seconds,  dtype: int64** |

Plotting this data will only display a linearly increasing graph not indicating that inter-day timesplits could be closer than intra-day timesplits.

{{< figure src="./linear_time.png" title="Time Features: linear plot of times without expected midnight gap." lightbox="true" >}}

## Sinus-Cosinus Transformation of Time

To obtain the desired behaviour of correctly interpreted time intervals at the transition between two days and thus correct cyclical characteristic representation, we represent times as sine/cosine value tuples. The following code will create a pandas dataframe from the former _times_ series and will

```python
def sine_cosine_transform(times):
    sine_time = np.sin(2 * np.pi * times / SECONDS_IN_DAY)
    cosine_time = np.cos(2 * np.pi * times / SECONDS_IN_DAY)
    
    return pd.DataFrame({"sine_time": sine_time,
                         "cosine_time": cosine_time
                        })

cyclic_times = sine_cosine_transform(times)
cyclic_times.head()
```

generate an output similar to this table.

I | sine_time | cosine_time
--| ----------| ------------
0 | 0.000070  | 1.000000
1 | 0.000209  | 1.000000
2 | 0.003072  | 0.999995
3 | 0.004468  | 0.999990
4 | 0.004887  | 0.999988

Plotting the calculated sine and cosine values will yield well known plots.

```python
cyclic_times.sine_time.plot()
cyclic_times.cosine_time.plot()
```

{{< figure src="./sine_cos_times.png" title="Time Features: Sine and cosine plot of time points." lightbox="true" >}}

Be aware that you cannot use a single value alone, e.g. sine or cosine, for representing time as you would introduce symmetric effects as horizontal lines drawn accross the plot would always touch two points in the graph with the meaning that two non-adjacent times would be equal.

Using _sine_time_ and _cosine_time_ together creates the necessary unambiguity and introduces cyclic properties correctly as machine-learning feature. The following graph of a randomly selected sample of _sine-cosine-time_ points illustrates the properties of the previous feature transformations. 

```python
cyclic_times.sample(75).plot.scatter('sine_time','cosine_time').set_aspect('equal');
```

{{< figure src="./circular_times.png" title="Time Features: Circular plot of sine-cosine time points." lightbox="true" >}}

Have fun evaluating the effects of your transformed cyclic values on your machine learning models.

Kind regards,

Henrik Hain
