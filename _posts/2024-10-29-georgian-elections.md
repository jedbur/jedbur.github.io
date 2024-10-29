---
layout: post
title: Identifying statistical irregularities in the 2024 Georgian Parliamentary Elections
date: 2024-10-29 18:00:00
description:
tags: 
categories: 
---
&nbsp;

The 2024 Georgian Parliamentary elections were held last Saturday, October 26th. Since the moment polls opened, numerous accounts of malpractice have been reported: international observers have identified instances of vote-buying, ballot stuffing, election carousels, as well as intimidation and violence against the observers themselves. The opposition party and president Salome Zourabichvili have denied the results, and called for a re-run of the elections and an investigation into the irregularities. The ruling Georgian Dream (GD) party maintains everything was carried out fairly and honestly. 

Very often, fraudulent elections can be identified through various statistical 'fingerprints' in the reported results. The Georgian Central Election Comission (CEC) has made the results publically available, and there has been some excellent analysis done already; one popular observation (first made by Roman Udot [here](https://x.com/romanik_/status/1850634786018066728) and Levan Kvirkvelia [here](https://x.com/LevanKvirkvelia/status/1850761181599858792)) is that the distribution of support for the ruling party (expressed as the % of votes cast going to GD) across polling stations is almost -- but not quite -- a normal distribution, with a 'Russian tail' in the high-GD-support end of the curve: a pattern which suggests an abnormally high number of strongly pro-GD polling results. This is [especially prominent](https://x.com/romanik_/status/1850634766279962994) when the data from rural areas is isolated, i.e. polling stations from big cities like Tbilisi, Batumi or Kutaisi are excluded. By no coincidence, such rural regions are exactly where the majority of electoral fraud reports have come from. This is pretty much in line with what has been reported by independent observers, and I highly recommend reading the original threads for more details.

<div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="/assets/img/ge2024-1.jpeg" class="img-fluid rounded z-depth-1" zoomable=false width="80%" height="80%" %}
</div>
<div class="caption">
    Source: <a href="https://x.com/EuropeElects/status/1851183540991406458" > Europe Elects on X/Twitter. </a>
</div>

Intrigued by the discrepancies in rural & urban electoral data, I have carried out some further analysis, to see if we can find more smoking guns of fraud. The electoral dataset I used is available [here](https://www.electoral.graphics/en-us/Elections-and-Datasets/Navigator-for-Elections/with-datasets/georgia-the-parliament-2024), and the code I used to make the plots is available [on my github](https://github.com/jedbur/ge2024/). I am neither a statistician nor a data scientist, however the beauty of plots is that those more experienced than me can use them to draw their own conclusions. 

&nbsp;

### Government support vs. turnout

Scatter plots of ruling party support against turnout are one common way of identifying signs of voter fraud. To make one, take the reported results from every polling station, and plot each report as a dot, with voter turnout (votes cast / registered voters) on the $$x$$-axis, and ruling party support (votes for the ruling party / votes cast) on the $$y$$-axis.

In a fair election, we would typically expect no correlation between the two -- the proportion of votes each party gets should be (on average) the same regardless of how many people came out to vote in a particular constituency. However, observing a positive correlation between the two could suggest fraud took place: if the ruling party is stuffing boxes or sending people to vote multiple times, this will raise the turnout rate in the affected polling stations. Since the additional votes then mostly go to the cheating party, their support rate also increases. This technique has been used, for example, in [this political science paper](https://www.pnas.org/doi/10.1073/pnas.1210722109) (as well as [this one](https://doi.org/10.1111/j.1740-9713.2016.00936.x)) to detect systematic irregularities in elections. In the case of Russian elections, there is an additional artefact in the scatter plots. The below graphic from the Economist shows that on top of the strong turnout-support correlation, the scatter plot of two decades' worth of Russian electoral data features prominent 'gridlines', suggesting that polling stations systematically make up the reported vote totals to fit predetermined rates of turnout and support.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-3.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-2.png" class="img-fluid rounded z-depth-1" width="120%" height="120%" zoomable=true %}
    </div>
</div>

<div class="caption">
    Left: Figure taken from <a href="https://www.pnas.org/doi/10.1073/pnas.1210722109" >Klimek, Yegorov, Hanel, Thurner (2012). </a> Right: Figure by <a href="https://www.economist.com/graphic-detail/2021/10/11/russian-elections-once-again-had-a-suspiciously-neat-result"> The Economist. </a>
</div>

&nbsp;

#### Georgia 2024

We can look for similar 'comet' patterns in the Georgian electoral data. Below are some plots I made using Python. The first is a simple scatter plot (like the one in The Economist), and the other is a heatmap (made by dividing the scatter plot into a grid, and colouring each cell by the number of points it contains):

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-4.png"  class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-5.png" class="img-fluid rounded z-depth-1"  zoomable=true %}
    </div>
</div>

Though hard to articulate exactly, there seems to be a trend there: in one region of the plot, turnout looks correlated with GD vote share. In another (the lower 'blob'), there is an opposite effect. It turns out these trends are not a coincidence: when we separate out data from urban and rural areas, we see that ***rural areas almost fully account for the 'comet' pattern.*** The pattern is nowhere as extreme as what we see in Russian, or Ugandan elections -- but it is there:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-6.png"  class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-7.png" class="img-fluid rounded z-depth-1"  zoomable=true %}
    </div>
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-9.png"  class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-8.png" class="img-fluid rounded z-depth-1"  zoomable=true %}
    </div>
</div>
<div class="caption">
    Scatter plots (top) and heatmaps (bottom) of the Georgian electoral data, separated by urban (left) and rural (right) areas.
</div>

Of course, trends like this can be observed in fair elections too, whenever there is a strong and successful voting drive for a particular party. Usually, such trends are explainable by other trends observed around the election. In the case of Georgia, however, these patterns combined with the numerous reports of fraud seem to be a consistent story: ***it looks like Georgian Dream has been artificially inflating their vote share in rural areas.***

&nbsp;

### Benford's Law 

Another way to look for irregularities in election data is to check whether it abides by Benford's Law. This is a statistical rule that many naturally occuring datasets follow, and it states that the leading (leftmost) digits of numbers in the dataset appear with specific probabilities ($$P(d) = \log_{10} ( 1 + 1/d)$$ where $$1 \leq d \leq 9$$ is the leading digit). In checking whether the Georgian electoral data obeys this, I largely followed [this blogpost](https://towardsdatascience.com/how-i-tested-the-hungarian-election-for-fraud-using-benfords-law-2d32ea92fe7c) by Jens Ringsholm who tested the Hungarian election for fraud. Because voting numbers from individual polling stations tend to be quite small (Benford's law requires that the relevant data spans several orders of magnitude), I aggregated the CEC polling data by the District ID, and tallied the leading digit in the total vote count for each party for each district. The overall result is below:

<div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="/assets/img/ge2024-12.png" class="img-fluid rounded z-depth-1" zoomable=false width="100%" height="100%" %}
</div>

As before, we can also compare the results from urban and rural areas:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-10.png"  class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-11.png" class="img-fluid rounded z-depth-1"  zoomable=true %}
    </div>
</div>

It mostly looks like the data follows Benford's Law. However, some political scientists (see [here](https://websites.umich.edu/~wmebane/fraud06.pdf) for an example) also advocate for second digit analysis, i.e. checking that the ***second*** digits of numbers in the electoral dataset appear with the correct frequencies. The relevant plots for this are below:

<div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="/assets/img/ge2024-15.png" class="img-fluid rounded z-depth-1" zoomable=false width="100%" height="100%" %}
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-14.png"  class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="/assets/img/ge2024-13.png" class="img-fluid rounded z-depth-1"  zoomable=true %}
    </div>
</div>

To me, for the rural data it mostly looks like the second digit distribution also adheres to Benford's law. This does not rule out fraud, and in fact ballot stuffing can in principle lead to a 'naturally' distributed dataset. On the other hand, there seems to be something strange going on in urban areas, where second digits '1' and '4' are severely overrepresented, whilst '3', '5' and '7' are underrepresented. In my view, with 318 data points this is not necessarily a statistical fluke, and could be indicative of some kind of manipulation. It is far beyond my expertise to investigate it further, but I hope somebody with experience will take a better look. 

&nbsp;

Again, I am not a researcher, but I hope the above manages to raise awareness about what's going on in Georgia, and that it might inspire others to take a closer look at the rural vs. urban trends. Jupyter notebooks used to make the plots are freely available on my [github](https://github.com/jedbur/ge2024/); doing significance tests of the above findings might be a good way to proceed further.  