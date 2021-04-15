### Using which_dirty() and why_dirty() to explore status of pipeline targets

In the image contained within the previous comment, you may have noticed the faded fill color of the target shapes. That styling signifies that the targets are out of date ("dirty") or haven't been created yet. 

We've put some fragile elements in the pipeline that will be addressed later, but if you were able to muscle through the failures with multiple calls to `scmake()`, you likely were able to build the figure near the end of the dependency diagram. For this example, we'll stop short of building the `"3_visualize/out/figure_1.png"` target by calling `scmake('site_data_styled')` instead to illustrate the concept of a dirty target. 

#### Which targets are incomplete/dirty?

The updated `remake::diagram()` output looks like this:
![image](https://user-images.githubusercontent.com/2349007/82731263-29b14900-9ccb-11ea-81ad-a35fedd09be2.png)

Only the colors have changed from the last example, signifying that the darker targets are "complete", but that `"3_visualize/out/figure_1.png"` and the two `data.csv` files still don't exist. 

the `scipiper` package has a useful function called `which_dirty()` which will list the incomplete ("dirty") targets that need to be updated in order to satisfy the output (once again, the default for this function is to reference the `all` target).

```r
which_dirty()
[1] "1_fetch/out/nwis_01427207_data.csv" "3_visualize/out/figure_1.png"       "all"                
```
This output tells us the same thing as the visual, namely that these three targets :point_up: are incomplete/dirty. No information is shared for the `"1_fetch/out/nwis_01435000_data.csv"` target, even though it is out of date (and would be considered "dirty" as well). This is because that target is not relevant to building `all`.

---

#### Why are these targets dirty?

Calling `why_dirty()` on a single target tells us a number of important things
```
why_dirty("3_visualize/out/figure_1.png")
The target '3_visualize/out/figure_1.png' does not exist
# A tibble: 4 x 8
  type     name                         hash_old hash_new                         hash_mismatch dirty dirty_by_descent current
  <chr>    <chr>                        <chr>    <chr>                            <lgl>         <lgl> <lgl>            <lgl>  
1 target   3_visualize/out/figure_1.png none     none                             FALSE         TRUE  FALSE            FALSE  
2 depends  site_data_styled             NA       183e7990d33bbc76314aa48f04e58531 NA            FALSE FALSE            TRUE   
3 fixed    NA                           NA       96653adafc4622c2088c81ea947966af NA            FALSE FALSE            TRUE   
4 function plot_nwis_timeseries         NA       4592eea358bfd73d90bee824dda0e0c7 NA            FALSE FALSE            TRUE   
```

From this output, with a little help from the `?scipiper::why_dirty()` documentation, it is clear that `"3_visualize/out/figure_1.png"` is "dirty" for several reasons:
- The target doesn't yet exist, which is why `hash_old` and `hash_new` are both `"none"`; the function also printed a message _The target '3_visualize/out/figure_1.png' does not exist_
- All other depedencies of the figure are up to date, including the `site_data_styled` target, the `plot_nwis_timeseries` function, and the `"fixed"` inputs to the function (which for this target, include `width = 12`, `height = 7`, and `units = 'in'`)

A build of the figure with `scipiper::scmake('3_visualize/out/figure_1.png')` will update the target dependencies, result in a `remake::diagram()` output which darkens the fill color on the `"3_visualize/out/figure_1.png"`, and cause a call to `why_dirty("3_visualize/out/figure_1.png")` to result in a descriptive error letting the user know the target is not dirty. 

---

The target will be out of date if there are any modifications to the upstream dependencies (follow the arrows in the diagram "upstream") or to the function `plot_nwis_timeseries()`. Additionally, a simple update to the value of one of the `"fixed"` arguments will cause the `"3_visualize/out/figure_1.png"` target to be "dirty". Here the `height` argument was change from 7 to 8) 
```
why_dirty("3_visualize/out/figure_1.png")
Since the last build of the target '3_visualize/out/figure_1.png':
  * the fixed arguments (character, logical, or numeric) to the target's command have changed
# A tibble: 4 x 8
  type     name                         hash_old                         hash_new                         hash_mismatch dirty dirty_by_descent current
  <chr>    <chr>                        <chr>                            <chr>                            <lgl>         <lgl> <lgl>            <lgl>  
1 target   3_visualize/out/figure_1.png bfdee70b50f05636b06ad32ef3b11810 bfdee70b50f05636b06ad32ef3b11810 FALSE         TRUE  FALSE            FALSE  
2 depends  site_data_styled             183e7990d33bbc76314aa48f04e58531 183e7990d33bbc76314aa48f04e58531 FALSE         FALSE FALSE            TRUE   
3 fixed    NA                           96653adafc4622c2088c81ea947966af 82eb1fa6b001fe33b8af3c8a629421a8 TRUE          FALSE FALSE            TRUE   
4 function plot_nwis_timeseries         4592eea358bfd73d90bee824dda0e0c7 4592eea358bfd73d90bee824dda0e0c7 FALSE         FALSE FALSE            TRUE 
```

The "hash" of these three fixed input arguments _was_ "96653adafc4622c2088c81ea947966af" and is now "82eb1fa6b001fe33b8af3c8a629421a8", resulting in a `"hash_mismatch"` and causing `"3_visualize/out/figure_1.png"` to be "dirty". You'll hear more about hashes in the future, but for now, think of a hash as a string that is unique for any unique data. In the case of fixed arguments, changing the argument names, values, _or even the order they are specified_ will create a different hash value and cause the output target to be considered dirty. 

---

:keyboard: using `which_dirty()` and `why_dirty()` can reveal unexpected connections between the target and the various dependencies. Comment on some of the different information you'd get from `why_dirty()` that wouldn't be available in the visual produced with `remake::diagram()`.

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>
