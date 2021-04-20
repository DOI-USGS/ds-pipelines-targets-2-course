
### Targets

"Targets" are the main things that the `targets` package interacts with (if the name hadn't already implied that :zany_face:). They represent things that are made (they're also the vertices of the [dependency graph](http://en.wikipedia.org/wiki/Dependency_graph)). If you want to make a plot called `plot.pdf`, then that's a target. If you depend on a dataset called `data.csv`, that's a target (even if it already exists).

In `targets`, there are two main types:

* **files**: These are the targets that need to have `format = "file"` added as an argument to `tar_target()` and their command must return the filepath(s). We have learned that file targets can be single files, a vector of filepaths, or a directory. While this isn't a requirement for it to function, USGS Data Science workflows try to name file targets using their base name and including their file extension, e.g. the target for `"1_fetch/out/data.csv"` would be `"data_csv"`. 
* **objects**: These are R objects that represent intermediate objects in an analysis. However, these objects are transparently stored to disk so that they persist across R sesssions. Unlike actual R objects though they won't appear in your workspace and a little extra work is required to get at them (run `tar_load(target_name)`). The default is for `targets` to store these as `.RDS` files. There are other formats that can be used to store the intermediate objects but they are beyond the scope of this course.

---
:keyboard: Activity: Assign yourself to this issue to get started.

<hr>
<h3 align="center">I'll sit patiently until you've assigned yourself to this one.</h3>