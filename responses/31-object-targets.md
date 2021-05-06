### More details on object targets

As stated above, object targets are R objects that represent intermediate objects in an analysis.

Object targets are common in the example pipelines we have shown before. They are distinguished from file targets in the following ways:
- The target name does not have a file extension (e.g., "_csv") and resembles an R variable name (because that is basically what the object target is)
- The function that creates the target returns some data to generate the target as opposed to creating or appending to a file, e.g., with `write_csv`, `ggsave`, `write_feather`, `nc_create`, etc. The return value of a function is either the value of the last expression in the function or the argument to a call to `return()`.

These objects are often used because they offer a brevity advantage over files (e.g., you don't need to pass in a filename to the function) and preserve the classes and formatting of the data, which makes it a bit easier to keep dates, factors, and other special data types from changing when you write - and then later read in - a file (such as a .csv). Objects also give you the illusion that they aren't taking up space in your project directory and make workspaces look a bit tidier.

The "illusion" :tophat::rabbit: of objects not taking up space is because behind the scenes, these objects are actually written to file (.rds files, to be specific). You can see what exists under the hood with `dir('_targets/objects')`. The default is for `targets` to store these as `.rds` files. There are other formats that can be used to store the intermediate objects; if you're curious, check out the documentation for the `format` argument to `tar_target()`.

You can take a look at that same object referenced in {{ repoUrl }}/issues/{{ store.many_targets_id }} by using
```r
readRDS('_targets/objects/map.config')
$missing_data
[1] "grey90"

$plot_CRS
[1] "+init=epsg:2163"

$wfs
[1] "http://cida.usgs.gov/gdp/geoserver/wfs"

$feature
[1] "derivative:wbdhu8_alb_simp"

$countBins
 [1]    0    1    2    5   10   20   50  100  200  500 1000
 ```

(Not as convenient as accessing the data with `tar_read('map.config')` instead, which is what we'd recommend).

---
:keyboard: Add a comment to this issue so we know you're ready to continue learning

<hr>
<h3 align="center">I'll sit patiently until you've added a comment to this issue.</h3>
