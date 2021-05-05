### Creating side-effect targets or undocumented inputs

Moving into a pipeline-way-of-thinking can reveal some suprising habits you created when working under a different paradigm. Moving the work of scripts into functions is one thing that helps compartmentalize thinking and organize data and code relationships, but smart pipelines require even more special attention to how functions are designed. 

#### Side-effect targets
It is tempting to build functions that do several things; perhaps a plotting function also writes a table, or a data munging function returns a data.frame, but also writes a log file. You may have noticed that there is no easy way to specify two ouputs from a single function in a pipeline recipe. We can have multiple files as _inputs_ into a function/command, but only one output/target. If a function writes a file that is not explicitly connected in the recipe (i.e., it is a "side-effect" output), the file is untracked by the dependency manager, and treated like an **implicit** file target (i.e., a target which has no `"command"` to specify how it is built; future issues will cover more details on this type of target). If the side-effect file is relied upon by a later target, changes to the side-effect target will indeed trigger a rebuild of the downstream target, but the dependency manager will have no way of knowing when the side-effect target itself should be rebuilt. :no_mobile_phones:

Maybe the above doesn't sound like a real issue, since the side-effect target would be updated every time the other **explicit** target it is paired with is rebuilt. But this becomes a scary problem (and our first real gotcha!) if the **explicit** target is not connected to the critical path of the final sets of targets you want to build, but the **implicit** side-effect target is. What this means is that _even if the explicit target is out of date_, it will not be rebuilt because building _this_ target is unnecessary to completing the final targets (remember "skip the work you don't need" :arrow_right_hook:). The dependency manager doesn't know that there is a hidden rule for updating the side-effect target and that this update is necessary for assuring the final targets are up-to-date and correct. :twisted_rightwards_arrows:

Side-effect targets can be used effectively, but doing so requires a good understanding of implications for tracking them and advanced strategies on how to specify rules and dependencies in a way that carries them along. :ballot_box_with_check:

#### Undocumented inputs
---

Additionally, it is tempting to code a filepath within a function which has information that needs to be accessed in order to run. This seems harmless, since functions are tracked by the dependency manager and any changes to those will trigger rebuilds, right? Not quite. If a filepath like `"1_fetch/in/my_metadata.csv"` is specified as an argument to a function but is not also a target in the makefile recipe, any changes to the `"1_fetch/in/my_metadata.csv"` will go unnoticed by the dependency manager, since the string that specifies the file name remains unchanged. The system isn't smart enough to know that it needs to check whether that file has changed. 

To depend on an input file, you need to set up a simple target that returns the filepath and use the target name (`my_metadata_csv` in our example) as input to targets that depend on it.

```
tar_target(my_metadata_csv, "1_fetch/in/my_metadata.csv", format = "file")
```

As a rule, unless you are purposefully trying to hide changes in a file from the dependency manager, do not put filepaths in the body of a function. :end:

---

:keyboard: Add a comment when you are ready to move on

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>
