### How (not to) depend on a directory for changes

You might have a project where there is a directory :file_folder: with a collection of files. To simplify the example, assume all of the files are `.csv` and have the same format. As part of the hypothetical project goals, these files need to be combined and formatted into a single plottable data.frame. 

In a data pipeline, we'd want assurance that any time the number of files changes, we'd rebuild the resulting data.frame. Likewise, if at any point the _contents_ of any one of the files changes, we'd also want to re-build the data.frame. 

This hypothetical example could be coded as 

```r
library(targets)
source("combine_files.R")

list(
  tar_target(in_files, 
             c('1_fetch/in/file1.csv',
               '1_fetch/in/file2.csv',
               '1_fetch/in/file3.csv'), 
             format = "file"),
  tar_target(
    plot_data, 
    combine_into_df(in_files)
  ),
  tar_target(figure_1_png, my_plot(plot_data))
)
```

While this solution would work, it is less than ideal because it doesn't scale well to **many** files, nor would it adapt to new files coming into the `1_fetch/in` directory :file_folder: (the pipeline coder would need to manually add file names to the `in_files` target).

---

Lucky for us, the `targets` package can handle having a directory as a target. If you add a target for a directory, the pipeline will track changes to the directory and will rebuild if it detects changes to the contents a file, the name of a file, or the number of files in the directory changes. 

To track changes to a directory, add the directory as a file target (see the `in_dir` target below). Important - you must add `format = "file"`! Then, you can use that directory as input to other functions. Note that you'd also need to modify your `combine_into_df` function to use `dir(in_dir)` to generate the file names since `in_dir` is just the name of the directory. 
```r
library(targets)
source("combine_files.R")

list(
  tar_target(in_dir, '1_fetch/in', format = "file"),
  tar_target(
    plot_data, 
    combine_into_df(in_dir)
  ),
  tar_target(figure_2_png, my_plot(plot_data))
)
```

Yay! :star2: This works because a change to any one of the files (or an addition/deletion of a file) in `1_fetch/in` will result in a rebuild of `in_dir`, which would cause a rebuild of `plot_data`. 

---

:keyboard: Add a comment when you are ready to move on.  

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>
