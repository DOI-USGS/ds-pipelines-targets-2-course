### Visualizing and understanding the status of dependencies in a pipeline

Seeing the structure of a pipeline as a visual is powerful. Viewing connections between targets and the direction data is flowing in can help you better understand the role of pipelines in data science work. Once you are more familiar with pipelines, using the same visuals can help you diagnose problems. 

Below is a makefile that is very similar to the one you have in your code repository (the option configurations and `source` calls were removed for brevity, but they are unchanged):
```r

p1_fetch <- list(
  tar_target(
    site_data,
    download_nwis_data(),
  ),
  tar_target(
    site_info_csv,
    nwis_site_info(fileout = "1_fetch/out/site_info.csv", site_data),
    format = "file"
  ),
  tar_target(
    nwis_01427207_data_csv,
    download_nwis_site_data('1_fetch/out/nwis_01427207_data.csv'),
    format = "file"
  ),
  tar_target(
    nwis_01435000_data_csv,
    download_nwis_site_data('1_fetch/out/nwis_01435000_data.csv'),
    format = "file"
  )
)

p2_process <- list(
  tar_target(
    site_data_clean, 
    process_data(site_data)
  ),
  tar_target(
    site_data_annotated,
    annotate_data(site_data_clean, site_filename = site_info_csv)
  ),
  tar_target(
    site_data_styled,
    style_data(site_data_annotated)
  )
)

p3_visualize <- list(
  tar_target(
    figure_1_png,
    plot_nwis_timeseries(fileout = "3_visualize/out/figure_1.png", site_data_styled),
    format = "file"
  )
)

```

Two file targets (`nwis_01427207_data_csv` and `nwis_01435000_data_csv`) were added to this makefile, but there were no changes to the functions, since `download_nwis_site_data()` already exists and is used to create a single file that contains water monitoring information for a single site. 

---

#### tar_glimpse()

The `targets` package has a nice function called `tar_glimpse()` that we haven't covered yet. It produces a dependency diagram for the target(s) you pass to the `allow` argument (it will show all of them by default). For this _modified_ makefile, calling that function with the default arguments produces:
```r
targets::tar_glimpse()
```
![glimpse_diagram](https://user-images.githubusercontent.com/13220910/115287075-74bf2080-a115-11eb-87d0-36107599ff2f.png)

If you run the same command, you'll see something similar but the two new files won't be included. 

---

Seeing this diagram helps develop a greater understanding of some of the earlier concepts from [intro-to-targets-pipelines](https://lab.github.com/USGS-R/intro-to-targets-pipelines). Here, you can clearly see the connection between `site_data` and `figure_1_png`. The figure_1 plot _needs_ all of the previous steps to have run in order to build. The arrows communicate the connections (or "dependencies") between targets, and if a target doesn't have any arrows connected to it, it isn't depended _on_ by another target and it doesn't depend _on_ any another targets. The two new .csv files are both examples of this, and in the image above they are floating around with no connections. A floater target like these two will still be built by `tar_make()` if they are included in the final target list (e.g., here they appear in `p1_fetch` which is included in the final target list returned at the end of `_targets.R`)

The diagram also shows how the inputs of one function create connections to the output of that function. `site_data` is used to build `site_data_clean` (and is the only input to that function) and it is also used as an input to `"1_fetch/out/site_info.csv"`, since the `nwis_site_info()` function needs to know what sites to get information from. These relationships result in a split in the dependency diagram where `site_data` is directly depended on by two other targets. 

---

#### tar_manifest()

Another useful technique for examining your pipeline connections is to use `tar_manifest()`, which returns a data.frame of information about the targets. While visual examination gives a complete overview, sometimes it is also useful to have programmatic access to your target names. Below is the table that is returned from `tar_manifest()` (remember that yours might be slightly different because it won't include the two new files). 

```r
tar_manifest()

# A tibble: 8 x 3
  name                  command                                                                            pattern
  <chr>                 <chr>                                                                              <chr>  
1 site_data             "download_nwis_data()"                                                             NA     
2 nwis_01435000_data_c~ "download_nwis_site_data(\"1_fetch/out/nwis_01435000_data.csv\")"                  NA     
3 nwis_01427207_data_c~ "download_nwis_site_data(\"1_fetch/out/nwis_01427207_data.csv\")"                  NA     
4 site_data_clean       "process_data(site_data)"                                                          NA     
5 site_info_csv         "nwis_site_info(fileout = \"1_fetch/out/site_info.csv\",  \\n     site_data)"      NA     
6 site_data_annotated   "annotate_data(site_data_clean, site_filename = site_info_csv)"                    NA     
7 site_data_styled      "style_data(site_data_annotated)"                                                  NA     
8 figure_1_png          "plot_nwis_timeseries(fileout = \"3_visualize/out/figure_1.png\",  \\n     site_d~ NA 
```

---

:keyboard: comment on what you learned from exploring `tar_glimpse()` and `tar_manifest()`.

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>

