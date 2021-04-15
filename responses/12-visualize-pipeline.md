### Visualizing and understanding the status of dependencies in a pipeline

Seeing the structure of a pipeline as a visual is powerful. Viewing connections between targets and the direction data is flowing in can help you better understand the role of pipelines in data science work. Once you are more familiar with pipelines, using the same visuals can help you diagnose problems. 

Below is a remakefile that is very similar to the one you have in your code repository (the `packages` and `sources` fields were removed for brevity, but they are unchanged):
```yaml
targets:
  all:
    depends: 3_visualize/out/figure_1.png

  site_data:
    command: download_nwis_data()
  
  1_fetch/out/site_info.csv:
    command: nwis_site_info(fileout = '1_fetch/out/site_info.csv', site_data)
  
  1_fetch/out/nwis_01427207_data.csv:
    command: download_nwis_site_data('1_fetch/out/nwis_01427207_data.csv')
  
  1_fetch/out/nwis_01435000_data.csv:
    command: download_nwis_site_data('1_fetch/out/nwis_01435000_data.csv')

  site_data_clean:
    command: process_data(site_data)

  site_data_annotated:
    command: annotate_data(site_data_clean, site_filename = '1_fetch/out/site_info.csv')

  site_data_styled:
    command: style_data(site_data_annotated)	

  3_visualize/out/figure_1.png:
    command: plot_nwis_timeseries(fileout = '3_visualize/out/figure_1.png', 
      site_data_styled, width = 12, height = 7, units = I('in'))
```

Two file targets (`"1_fetch/out/nwis_01427207_data.csv"` and `"1_fetch/out/nwis_01435000_data.csv"`) were added to this remakefile, but there were no changes to the functions, since `download_nwis_site_data()` already exists and is used to create a single file that contains water monitoring information for a single site. 

---

#### remake::diagram()

The `remake` package has a nice function called `diagram()` that we haven't covered yet. It produces a dependency diagram for the target(s) you specify (remember that `all` is the default target). For this _modified_ remakefile, calling that function with the default arguments produces:
```r
remake::diagram()
```
![remake_diagram](https://user-images.githubusercontent.com/2349007/82730676-1bf9c480-9cc7-11ea-8722-e9ffb3bbfa2f.png)

If you run the same command, you'll see something similar but the two new files won't be included. 

---

Seeing this diagram helps develop a greater understanding of some of the earlier concepts from [intro-to-pipelines](https://lab.github.com/USGS-R/intro-to-pipelines). Here, you can clearly see the connection between `all` and `"3_visualize/out/figure_1.png"`. The figure_1 plot _needs_ to be created in order to complete `all`. The arrows communicate the connections (or "dependencies") between targets, and if a target doesn't have any arrows connected to it, it isn't depended _on_ by another target and it doesn't depend _on_ any another targets. The two new .csv files are both examples of this, and in the image above they are floating around with no connections. A floater target like these two won't be built by `scmake()` unless it is called directly (e.g., `scmake("1_fetch/out/nwis_01427207_data.csv")`; remember again "skip the work you don't need"). 

The diagram also shows how the inputs of one function create connections to the output of that function. `site_data` is used to build `site_data_clean` (and is the only input to that function) and it is also used as an input to `"1_fetch/out/site_info.csv"`, since the `nwis_site_info()` function needs to know what sites to get information from. These relationships result in a split in the dependency diagram where `site_data` is directly depended on by two other targets. 

By modifying the recipe for the all target, it is possible to create a dependency link to one of the .csv files, which would then result in that file being included in a build as it becomes necessary in order to complete `all`:
```yaml
targets:
  all:
    depends: ["3_visualize/out/figure_1.png", "1_fetch/out/nwis_01427207_data.csv"]
```

And after calling `remake::diagram()`, it is clear that now `"1_fetch/out/nwis_01427207_data.csv"` has found a home and is relevant to `all`!
![remake_diagram_update](https://user-images.githubusercontent.com/2349007/82730950-3765cf00-9cc9-11ea-9da7-fbb6f0538b33.png)

With this update, the build of `"1_fetch/out/nwis_01427207_data.csv"` would no longer be skipped when `scmake()` is called. 

---

:keyboard: comment on what you learned from exploring `remake::diagram()`

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>


