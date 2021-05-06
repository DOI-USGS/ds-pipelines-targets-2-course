## How to make decisions on how many targets to use and how targets are defined

We've covered a lot of content about the rules of writing good pipelines, but pipelines are also very flexible! Pipelines can have as many or as few targets as you would like, and targets can be as big or as small as you would like. The key theme for all pipelines is that they are reproducible codebases to document your data analysis process for both humans and machines. In this next section, we will learn about how to make decisions related to the number and types of targets you add to a pipeline.

### background 
Isn't it satisfying to work through a fairly lengthy data workflow and then return to the project and it _just works_? For the past few years, we have been capturing the steps that go into creating results, figures, or tables appearing in data visualizations or research papers. There are recipes for reproducibility used in complex, collaborative modeling projects, such as in [this reservoir temperature modeling pipeline](https://code.usgs.gov/wma/wp/res-temperature-process-models) and in [this pipeline to manage downloads of forecasted meteorological driver data](https://code.usgs.gov/wma/wp/forecasted-met-drivers). _Note that you need to be able to access internal USGS websites to see these examples and these were developed early on in the Data Science adoption of `targets` so may not showcase all of our adopted best practices_.

---

Here is a much simpler example that was used to generate **Figure 1** from [Water quality data for national‐scale aquatic research: The Water Quality Portal](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2016WR019993) (published in 2017):

```r
library(targets)

## All R files that are used must be listed here:
source("R/get_mutate_HUC8s.R")
source("R/get_wqp_data.R")
source("R/plot_huc_panel.R")

tar_option_set(packages = c("dataRetrieval", "dplyr", "httr", "lubridate", "maps",
                            "maptools", "RColorBrewer", "rgeos", "rgdal", "sp", "yaml"))

# Load configuration files
p0_targets_list <- list(
  tar_target(map_config_yml, "configs/mapping.yml", format = "file"),
  tar_target(map_config, yaml.load_file(map_config_yml)),
  tar_target(wqp_config_yml, "configs/wqp_params.yml", format = "file")
  tar_target(wqp_config, yaml.load_file(wqp_config_yml))
)

# Fetch data
p1_targets_list <- list(
  tar_target(huc_map, get_mutate_HUC8s(map_config)),
  tar_target(phosphorus_lakes, get_wqp_data("phosphorus_lakes", wqp_config, map_config)),
  tar_target(phosphorus_all, get_wqp_data("phosphorus_all", wqp_config, map_config)),
  tar_target(nitrogen_lakes, get_wqp_data("nitrogen_lakes", wqp_config, map_config)),
  tar_target(nitrogen_all, get_wqp_data("nitrogen_all", wqp_config, map_config)),
  tar_target(arsenic_lakes, get_wqp_data("arsenic_lakes", wqp_config, map_config)),
  tar_target(arsenic_all, get_wqp_data("arsenic_all", wqp_config, map_config)),
  tar_target(temperature_lakes, get_wqp_data("temperature_lakes", wqp_config, map_config)),
  tar_target(temperature_all, get_wqp_data("temperature_all", wqp_config, map_config)),
  tar_target(secchi_lakes, get_wqp_data("secchi_lakes", wqp_config, map_config)),
  tar_target(secchi_all, get_wqp_data("secchi_all", wqp_config, map_config)),
)

# Summarize the data in a plot
p2_targets_list <- list(
  tar_target(
    multi_panel_constituents_png,
    plot_huc_panel(
      "figures/multi_panel_constituents.png", huc_map, map_config, 
      arsenic_lakes, arsenic_all, nitrogen_lakes, nitrogen_all, 
      phosphorus_lakes, phosphorus_all, secchi_lakes, secchi_all, 
      temperature_lakes, temperature_all
    ),
    format = "file")
)

# Combine all targets into a single list
c(p0_targets_list, p1_targets_list, p2_targets_list)
```

This makefile recipe generates a multipanel map, which colors [HUC8 watersheds](http://dep.wv.gov/WWE/getinvolved/sos/Documents/Basins/HUCprimer.pdf) according to how many sites within the watershed have data for various water quality constituents:
![multi_panel_constituents](https://user-images.githubusercontent.com/2349007/82117369-18999280-9735-11ea-8365-e58742c5ff7e.png)

---

The `"figures/multi_panel_constituents.png"` figure takes a while to plot, so it is a somewhat "expensive" target to iterate on when it comes to style, size, colors, and layout (it takes 3 minutes to plot for me). But the plotting expense is dwarfed by the amount of time it takes to build each water quality data "object target", since `get_wqp_data` uses a web service that queries a large database and returns a result; the process of fetching the data can sometimes take over thirty minutes (`nitrogen_all` is a target that contains the locations of all of the sites that have nitrogen water quality data samples). 

Alternatively, the `map_config*` object above builds in a fraction of second, and contains some simple information that is used to fetch and process the proper boundaries with the `get_mutate_HUC8s` function, and includes some plotting details for the final map (such as plotting color divisions).

This example, although dated, represents a real project that caused us to think carefully about how many targets we use in a recipe and how complex their underlying functions are. Decisions related to targets are often motivated by the intent of the pipeline. In the case above, our intent at the time was to capture the data and processing behind the plot in the paper in order to satisfy our desire for reproducibility. 

---

:keyboard: Activity: Assign yourself to this issue to get started.

<hr>
<h3 align="center">I'll sit patiently until you've assigned yourself to this one.</h3>
