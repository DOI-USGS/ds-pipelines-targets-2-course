### What to do when you want to specify a non-target input to a command? 

Wow, we've gotten this far and haven't written a function that accepts anything other than an object target or a file target. I feel so constrained!

In reality, R functions have all kinds of other arguments, from logicals (TRUE/FALSE), to characters that specify which configurations to use. 

The example in your working pipeline creates a figure, called `3_visualize/out/figure_1.png`. Unless you've made a lot of modifications to the `plot_nwis_timeseries()` function, it has a few arguments that have default values, namely `width = 12`, `height = 7`, and `units = 'in'`. Nice, you can control your output plot size here!

We can add those to the makefile like so
```r
tar_target(
  figure_1_png,
  plot_nwis_timeseries(fileout = "3_visualize/out/figure_1.png", site_data_styled,
                       width = 12, height = 7, units = 'in'),
  format = "file"
)
```

and it works! :star2:

---

What if we wanted to specify the same plot sizes for multiple plots? We could pass in `width = 12`, `height = 7`, and `units = 'in'` each time `plot_nwis_timeseries` is called _OR_ we can create R objects in the makefile that define these configurations and use them for multiple targets. 

You can add these
```r
p_width <- 12
p_height <- 7
p_units <- "in"
```

to your `_targets.R` file and then call them in the plot command for your targets, such as
```r
tar_target(
  figure_1_png,
  plot_nwis_timeseries(fileout = "3_visualize/out/figure_1.png", site_data_styled,
                       width = p_width, height = p_height, units = p_units),
  format = "file"
)
```

Objects used in the command for `tar_target()` need to be created somewhere before they are called. It is usually a good idea to put configuration info near the top of the makefile. I would suggest adding the code to create these three objects immediately after your `tar_option_set()` call.

---

Another example of when this object (rather than target) pattern comes in handy is when we want to _force_ a target to rebuild. Note that in the example below, we are writing the `command` for this target by putting two lines of code between `{}` rather than calling a separate custom function. You can do this for any target, but it is especially useful in this application when we just have two lines of code to execute.

```r
library(targets)

list(
  tar_target(
    work_files,
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
By adding this `dummy` object to our `command` argument for the `work_files` target, we can modify the dummy contents any time we want to force the update of `work_files`. Updating the `dummy` argument to today's date allows us to simultaneously force the update and record when we last downloaded the data from ScienceBase. You may see the use of these `dummy` arguments in spots where there is no other trigger that would cause a rebuild, such as pulling data from a remote webservice or website when `targets` has no way of knowing that new data are available on the same service URL.

---

:keyboard: Add a comment when you are ready to move on.  

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>
