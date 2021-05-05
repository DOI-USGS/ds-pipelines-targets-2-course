
So far you’ve learned a lot about the mechanics of using **targets**, but there are also a few conventions that USGS Data Science practitioners use to maintain consistency across projects. These conventions make it easier to jump into a new project, provide peer review, or learn a new technique from someone else’s pipeline, since you are already familiar with the structure.

---

As you learned in the first pipelines course, we like to separate pipelines into distinct phases based on what is happening to the data (we usually use `fetch`, `process`, `visualize`, etc). So far in this course, we have been using a single list of targets in the `_targets.R` makefile. This works for short pipelines, but when you have bigger, more complex pipelines, that file and target list could get HUGE and difficult to read. 

For this reason, we like to have multiple makefiles that each describe and are named after a single phase, e.g., `1_fetch.R` or `4_visualize.R`. Within each phase makefile, targets are saved in an R list object which is numbered based on the phase, e.g. `p1_targets` or `p4_targets`. Then, the main `_targets.R` makefile sources each of these phase makefiles and combines the target lists into a single list using `c()`, e.g., `c(p1_targets, p2_targets, p3_targets, p4_targets`.

In addition to this multi-makefile approach, we also like to name our targets to make it clear which phase they belong to. For example, any target created in the `p1_fetch` phase would be prefixed with `p1`. We do this for two reasons: 1) it is clearer, and 2) you can now use `dplyr::select` syntax to build all targets in a single phase by running `tar_make(starts_with("p1"))`. A handy little trick!

Consider the two-phased pipeline below, where you need to download data from ScienceBase and then combine it all into a single dataframe.

If the `1_fetch.R` makefile looked like this
```r
p1_fetch <- list(
  tar_target(
    p1_sb_files,
    {
      dummy <- '2021-04-19'
      item_file_download(sb_id = "4f4e4acae4b07f02db67d22b", 
                         dest_dir = "1_fetch/tmp",
                         overwrite_file = TRUE)
    },
    format = "file",
    packages = "sbtools"
  )
)
```

and the `2_process.R` makefile looked like this
```r
source("2_process/src/combine_files.R")

p2_process <- list(
  tar_target(
    p2_plot_data, 
    combine_into_df(sb_files)
  )
)
```

then the canonical`_targets.R` makefile would look like this
```r
library(targets)

source("1_fetch.R")
source("2_process.R")

# Return the complete list of targets
c(p1_fetch, p2_process)
```

You could then build the full pipeline by running `tar_make()`, or run specific phases using `tar_make(starts_with("p1"))` for the fetch phase and `tar_make(starts_with("p2"))` for the process phase.

---

:keyboard: Activity: Split your pipeline targets into the phases fetch, process, and visualize. Use a different makefile for each phase and follow our phase-naming conventions to name the makefiles and list objects. Also, rename your targets using the appropriate prefix (`p1_`, `p2_`, `p3_`). Run `tar_make()` and open a pull request. Paste your build status as a comment to the PR and assign Jordan or Alison as a reviewer. 

<hr>
<h3 align="center">I'll sit patiently until you open a new pull request</h3>
