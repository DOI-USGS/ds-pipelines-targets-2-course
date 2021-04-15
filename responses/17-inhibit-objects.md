### What to do when you want to specify a non-build-object input to a function? 

Wow, we've gotten this far and haven't written a function that accepts anything other than an object target or a file target. I feel so constrained!

In reality, R functions have all kinds of other arguments, from logicals (TRUE/FALSE), to characters that specify which configurations to use. 

The example in your working pipeline creates a figure, called `3_visualize/out/figure_1.png`. Unless you've made a lot of modifications to the `plot_nwis_timeseries()` function, it has a few arguments that have default values, namely `width = 12`, `height = 7`, and `units = 'in'`. Nice, you can control your output plot size here!

But adding those to the remake file like so
```yaml

  3_visualize/out/figure_1.png:
    command: plot_nwis_timeseries(target_name, site_data_styled, width = 12, height = 7, units = 'in')
```
causes an immediate error
```
scmake()
 Error in .remake_add_targets_implied(obj) : 
  Implicitly created targets must all be files:
 - in: (in 3_visualize/out/figure_1.png) -- did you mean: site_data, 1_fetch/out/site_info.csv, 
```
Since `'in'` is not an object target with a recipe in the pipeline, `remake` is trying to find the file corresponding to `"in"`, since it _must_ be a file if it isn't a number or an object.

We know `"in"` is not a file, it is instead a simple argument we want to expose in the recipe, so we can make modification. To do this, wrapping the argument in the `I()` function tells remake to "treat this argument 'as is'", meaning don't try to infer anything fancy, just pass it along to the function. 

```yaml

  3_visualize/out/figure_1.png:
    command: plot_nwis_timeseries(target_name, site_data_styled, width = 12, height = 7, units = I('in'))
```
works! :star2:

---

Going back to our previous example where we wanted to build a data.frame of file hashes from the '1_fetch/in' directory, `I()` comes in handy in two spots: 1) we can avoid the md5sum warning by wrapping the directory path in the `I()`, which means '1_fetch/in' is used as a real string instead of attempting to treat it as a file target, and 2) we can add a new variable that can help us _force_ or refresh the target with some information about when we last did it. 

```yaml
sources:
  - combine_files.R

targets:
  all:
    depends: figure_1.png
  
  work_files:
    command: hash_dir_files(directory = I('1_fetch/in'), dummy = I('2020-05-18'))
```
By adding this `dummy` argument to our own custom `hash_dir_files()` function, we can modify the dummy contents any time we want to force the update of `work_files`. Making a change to the text in the `dummy` argument has the same outcome as `scmake('work_files', force = TRUE)`, but this way we keep an easy to read record of when we last manually refreshed the pipeline's view of what exists within '1_fetch/in'. You may see the use of these `dummy` arguments in spots where there is no other trigger that would cause a rebuild (such as the case of these directory challenges or when pulling data from a remote webservice or website - `scipiper` has no way of knowing that new data are available on the same service URL).

---

:keyboard: Add a comment when you are ready to move on.  

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>