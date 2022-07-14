# Portfolio

## Large, collaborative software projects

**[oxView](https://github.com/sulcgroup/oxdna-viewer)** is a visualizer and editor for oxDNA files.  I wrote much of the original core functionality, however most of the recent updates have been done by other members of the team.  My most recent feature addition was the [RNA strand creator (begins on line 36)](https://github.com/sulcgroup/oxdna-viewer/blob/3b06a38d901c536358c221bbf64369071688f58b/ts/model/RNA.ts#L43) which solved a previous bug where RNA helices were created incorrectly.

**[oxDNA Analysis Tools](https://github.com/lorenzo-rovigatti/oxDNA/tree/master/analysis)** is a Python library for analyzing oxDNA simulations.  I recently did almost a complete rewrite of the entire library mostly on my own, but with some input on structures and file reading from Michael Matthies.  This new version of the library was included in the oxDNA package as the new default analysis tools replacing the old one.  If you are curious about how it works, I always direct new users to the [mean structure calculator](https://github.com/lorenzo-rovigatti/oxDNA/blob/master/analysis/src/oxDNA_analysis_tools/mean.py) because it is a very clean example of how to read in data and do some simple analysis.  We just finished the merge last week so the documentation is still under construction, but you can browse what is available [here](https://lorenzo-rovigatti.github.io/oxDNA/oat/index.html)

One of the most powerful aspects of the rewritten versions of both oxDNA and oxDNA Analysis Tools is that both now have import structures which work well with interactive Python notebooks, allowing users to create reproducible simulation and analysis pipelines which could look something like:
```python
# start an oxDNA simulation from an input text file
import oxpy

with oxpy.Context():
    inp = oxpy.InputFile()
    inp.init_from_filename("input")
    inp["print_conf_every"] = "5e5"
    manager = oxpy.OxpyManager(inp)
    manager.run(10000000)
```

```python
# Compute the mean structure over the configuration
from oxDNA_analysis_tools.mean import mean
from oxDNA_analysis_tools.deviations import deviations
from oxDNA_analysis_tools.UTILS.RyeReader import describe

top = inp["topology"]
traj = inp["trajectory_file"]
top_info, traj_info = describe(top, traj)

mean_conf = mean(traj_info, top_info, ncpus=4)
RMSDs, RMSFs = deviations(traj_info, top_info, mean_conf, ncpus=4)

RMSFs = {"RMSF": RMSFs.tolist()}
```

This can then be opened as an interactive oxView iFrame using a new interface developed by Michael:
```python
from oxDNA_analysis_tools.UTILS.oxview import oxdna_conf

oxdna_conf(top_info, mean_conf, RMSFs)
```
Which will produce an output which looks like:
![image](https://user-images.githubusercontent.com/36451772/178870785-2537771c-ff1a-45df-aa7a-7c2fe41f9315.png)


## Research projects
**[Leaf spring engines](https://github.com/sulcgroup/hinges)** demonstrates characterization of nanoscale leaf-spring engines using oxDNA.  The scripts in the scripts directory were used to analyze the more than 3 TB of simulation data characterizing both the equilibrium and out-of-equilibrium properties of this system with different design modifications.  This project is [available as a preprint](https://www.biorxiv.org/content/10.1101/2021.12.22.473833v1.abstract) which also shows experimental realization and characterization by our collaborators.

## Machine learning
oxDNA Analyis Tools contains [a script](https://github.com/lorenzo-rovigatti/oxDNA/blob/master/analysis/src/oxDNA_analysis_tools/clustering.py) for unsupervised clustering of MD trajectories, allowing automatic segmentation of a trajectory into subtrajectories which have a particular feature.

I was a content matter expert and performed all of the results analysis for [a project on using reinforcement learning to predict RNA structures](https://pubsonline.informs.org/doi/abs/10.1287/ijoc.2022.1188).  Work is ongoing on this project to see if more advanced classifiers than the support vector machine used in the original implementation (including GNNs) can improve the results, however I am not directly involved in the coding of the new models.
