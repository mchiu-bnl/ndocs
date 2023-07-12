# Using the ChargeSimEventVisualizer class

All dependencies for nexo-offline are available on Borax under: `/usr/gapps/nexo`

To configure your environment:
```
source /usr/gapps/nexo/setup.sh
```

This package also requires some python libraries:
* histlite
* pandas
* uproot

Note that if you're working on a cluster, you should install these in a `virtualenv`, since you
don't have admin permissions. 
[Instructions to do this at LLNL are here](https://hpc.llnl.gov/software/development-environment-software/python)
(see "Using virtualenv to Manage Your Own Python Environment").


## Set up paths

First, you need to ensure that the VisualizationTools directory is in your `PYTHONPATH`. 
You can set it up explicitly:
```
export PYTHONPATH=/path/to/nexo-offline/:$PYTHONPATH
```

## Quick example

I typyically work in Jupyter Notebooks, but you can also run it interactively using `ipython` like so:
```
[terminal_prompt]$ ipython

In [1]: import matplotlib.pyplot as plt

In [2]: from VisualizationTools import ChargeSimEventVisualizer 

In [3]:  vis = ChargeSimEventVisualizer.ChargeSimEventVisualizer()

In [4]: vis.LoadInputRootFile('../Analysis/g4andcharge_1000evts_xe127_seed2_1000msEL.root')

In [5]: vis.LoadTilesMap(input_tiles_map='/usr/gapps/nexo/nexo-offline/data/tilesMap_6mm.txt')

In [6]: fig, ax = vis.VisualizeEventXY( evt_num=7, \ 
   ...:                                 figure_num=1,\ 
   ...:                                 draw_tiles=True,\ 
   ...:                                 draw_nest_lineages=True,\ 
   ...:                                 figure_width=8,\ 
   ...:                                 figure_height=9 )

In [7]: plt.show()
```
You should see a plot pop up that looks like the example below:
![](example_chargesim_vis.png)


Note that the `VisualizeEventXY` does not run the function `plt.show()`, so the window will not pop 
up automatically (although it should be shown if you're running in a Jupyter notebook). 
Also, if you want to save the figure, use `plt.savefig()`.

Also, note that some of the plotting configuration parameters (i.e. fontsizes and stuff) are
currently hardcoded, which is not great. If it's annoying, let Brian know (or feel free to modify
it yourself).

 

