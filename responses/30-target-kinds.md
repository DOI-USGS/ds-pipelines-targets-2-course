`remake` is the R package that underlies many of `scipiper`'s functions. Here we've borrowed some text from the `remake` [github repo](https://github.com/richfitz/remake/blob/e29028b548950a3132ea2d045b7f67344ce22a6b/doc/concepts.md) (credit to [richfitz](https://github.com/richfitz), although we've lightly edited the original text) to explain differences between targets

### Targets

"Targets" are the main things that `remake` interacts with.  They represent things that are made (they're also the vertices of the [dependency graph](http://en.wikipedia.org/wiki/Dependency_graph)).  If you want to make a plot called `plot.pdf`, then that's a target.  If you depend on a dataset called `data.csv`, that's a target (even if it already exists).

There are several types of targets:

* **files**: The name of a file target is the same as its path. Something is actually stored in the file, and it's possible for the file contents to be modified outside of `remake` (files are the main types of targets that `make` deals with, since it is language agnostic). Within files, there are two sub-types:
  - *implicit*: these are file targets that are depended on somewhere in your process, but for which no rule to build them exists (i.e., there is no `command` in a remakefile). You can't build these of course. However, `remake` will build an implicit file target for them so it can internally monitor changes to that file.
  - *explicit*: these are the file targets that are built _by_ rules that were defined within your pipeline (i.e., command-to-target recipe exists in a remakefile).
* **objects**: These are R objects that represent intermediate objects in an analysis. However, these objects are transparently stored to disk so that they persist across R sesssions. Unlike actual R objects though they won't appear in your workspace and a little extra work is required to get at them.
* **fake**: Fake targets are simply pointers to other targets (in `make` these are "phoney" targets). The `all` depends on all the "end points" of your analysis is a "fake" target. Running `scmake("all")` will build all of your targets, or verify that they are up to date.

---
:keyboard: Activity: Assign yourself to this issue to get started.

<hr>
<h3 align="center">I'll sit patiently until you've assigned yourself to this one.</h3>