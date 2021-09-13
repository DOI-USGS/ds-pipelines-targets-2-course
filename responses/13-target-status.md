### Using tar_visnetwork() and tar_outdated() to explore status of pipeline targets

In the image contained within the previous comment, all of the shapes are circles of the same color. `tar_glimpse()` is useful to verify your pipeline connections, but once you start building your pipeline `tar_visnetwork()` creates a dependency diagram with more information and styles the shapes in ways to signify which targets are out of date or don't need to rebuild. 

We've put some fragile elements in the pipeline that will be addressed later, but if you were able to muscle through the failures with multiple calls to `tar_make()`, you likely were able to build the figure at the end of the dependency chain. For this example, we'll stop short of building the `figure_1_png` target by calling `tar_make('site_data_styled')` instead to illustrate an outdated target. 

#### Which targets are incomplete/outdated?

The output of `tar_visnetwork()` after running `tar_make('site_data_styled')` (and having never built all targets by running `tar_make()` with no inputs) looks like this:
![visnetwork](https://user-images.githubusercontent.com/13220910/133108278-d1095b74-b810-49a2-bdfb-310598e07c8b.png)

Only the colors have changed from the last example, signifying that the darker targets are "complete", but that `figure_1_png` and the two `data.csv` files still don't exist. 

The `targets` package has a useful function called `tar_outdated()` which will list the incomplete targets that need to be updated in order to satisfy the output (once again, the default for this function is to reference all targets in the pipeline).

```r
tar_outdated()
[1] "nwis_01432160_data_csv" "nwis_01427207_data_csv" "figure_1_png"                
```
This output tells us the same thing as the visual, namely that these three targets :point_up: are incomplete/outdated.

A build of the figure with `tar_make('figure_1_png')` will update the target dependencies, result in a `tar_visnetwork()` output which darkens the fill color on the `figure_1_png` shape, and cause a call to `tar_outdated("figure_1_png")` to result in an empty character vector, `character(0)`, letting the user know the target is not outdated. 

---

The `figure_1_png` target can become outdated again if there are any modifications to the upstream dependencies (follow the arrows in the diagram "upstream") or to the function `plot_nwis_timeseries()`. Additionally, a simple update to the value of one of the `"fixed"` arguments will cause the `figure_1_png` target to become outdated. Here the `height` argument was changed from 7 to 8:
```
tar_visnetwork("3_visualize/out/figure_1.png")
```

![visnetwork_fxnchange](https://user-images.githubusercontent.com/13220910/115302212-cd97b480-a127-11eb-9636-930ce7e02cb1.png)

In the case of fixed arguments, changing the argument names, values, _or even the order they are specified_ will create a change in the function definition and cause the output target to be considered outdated. Adding comments to the function code does not cause the function to be seen as changed.

---

:keyboard: using `tar_visnetwork()` and `tar_outdated()` can reveal unexpected connections between the target and the various dependencies. Comment on some of the different information you'd get from `tar_visnetwork()` that wouldn't be available in the output produced by `tar_glimpse()` or `tar_manifest()`.

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>
