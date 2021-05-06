## How to make decisions on how many targets to use and how targets are defined

### background 
Isn't it satisfying to work through a fairly lengthy data workflow and then return to the project and it _just works_? For the past few years, we have been capturing the steps that go into creating results, figures, or tables appearing in data visualizations or research papers. There are recipes for reproducibility used in complex, collaborative modeling projects, such as in [this reservoir temperature modeling pipeline](https://code.usgs.gov/wma/wp/res-temperature-process-models) and in [this pipeline to manage downloads of forecasted meteorological driver data](https://code.usgs.gov/wma/wp/forecasted-met-drivers). _Note that you need to be able to access internal USGS websites to see these examples and these were developed early on in the Data Science adoption of `targets` so may not showcase all of our adopted best practices_.

---

Here is a much simpler example that was used to generate **Figure 1** from [Water quality data for national‐scale aquatic research: The Water Quality Portal](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2016WR019993) (published in 2017):

```yaml

packages:
  - rgeos
  - dplyr
  - rgdal
  - httr
  - yaml
  - RColorBrewer
  - dataRetrieval
  - lubridate
  - maptools
  - rgeos
  - maps
  - sp
  
## All R files that are used must be listed here:
sources:
  - R/wqp_mapping_functions.R
  - R/readWQPdataPaged.R

targets:
  all:
    depends: 
      - figures/multi_panel_constituents.png
      
  map.config:
    command: yaml.load_file("configs/mapping.yml")
    
  wqp.config:
    command: yaml.load_file("configs/wqp_params.yml")
  
  huc.map:
    command: get_mutate_HUC8s(map.config)

  phosphorus_lakes:
    command: get_wqp_data(target_name, wqp.config, map.config)

  phosphorus_all:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  nitrogen_lakes:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  nitrogen_all:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  arsenic_lakes:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  arsenic_all:
    command: get_wqp_data(target_name, wqp.config, map.config)
  
  chlorophyll_lakes:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  chlorophyll_all:
    command: get_wqp_data(target_name, wqp.config, map.config)
  
  temperature_lakes:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  temperature_all:
    command: get_wqp_data(target_name, wqp.config, map.config)
  
  doc_lakes:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  doc_all:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  secchi_all:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  secchi_lakes:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  glyphosate_all:
    command: get_wqp_data(target_name, wqp.config, map.config)
    
  figures/multi_panel_constituents.png:
    command: plot_huc_panel(huc.map, map.config, target_name, arsenic_lakes, 
      arsenic_all, nitrogen_lakes, nitrogen_all, phosphorus_lakes, phosphorus_all, 
      secchi_lakes, secchi_all, temperature_lakes, temperature_all)
    plot: true
```

This remakefile recipe generates a multipanel map, which colors [HUC8 watersheds](http://dep.wv.gov/WWE/getinvolved/sos/Documents/Basins/HUCprimer.pdf) according to how many sites within the watershed have data for various water quality constituents:
![multi_panel_constituents](https://user-images.githubusercontent.com/2349007/82117369-18999280-9735-11ea-8365-e58742c5ff7e.png)

---

The `"figures/multi_panel_constituents.png"` figure takes a while to plot, so it is a somewhat "expensive" target to iterate on when it comes to style, size, colors, and layout (it takes 3 minutes to plot for me). But the plotting expense is dwarfed by the amount of time it takes to build each water quality data "object target", since `get_wqp_data` uses a web service that queries a large database and returns a result; the process of fetching the data can sometimes take over thirty minutes (`nitrogen_all` is a target that contains the locations of all of the sites that have nitrogen water quality data samples). 

Alternatively, the `map.config*` object above builds in a fraction of second, and contains some simple information that is used to fetch and process the proper boundaries with the `get_mutate_HUC8s` function, and includes some plotting details for the final map (such as plotting color divisions as specified by `countBins`):

![map.config build](https://user-images.githubusercontent.com/2349007/82117596-a629b200-9736-11ea-9118-b4391d5d4d39.png)


This example, although dated, represents a real project that caused us to think carefully about how many targets we use in a recipe and how complex their underlying functions are. Decisions related to targets are often motivated by the intent of the pipeline. In the case above, our intent at the time was to capture the data and processing behind the plot in the paper in order to satisfy our desire for reproducibility. 

---

*disclaimer, the code above was written at a time before we'd completely transitioned away from naming variables `like.this`


:keyboard: Activity: Assign yourself to this issue to get started.

<hr>
<h3 align="center">I'll sit patiently until you've assigned yourself to this one.</h3>