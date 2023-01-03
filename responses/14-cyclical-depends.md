### What are cyclical dependencies and how to avoid them?

Using `tar_visnetwork()` shows the dependency diagram of the pipeline. Look at previous comments to remind yourself of these visuals. 

As a reminder, the direction of the arrows capture the dependency flow, and `site_data` sits on the left, since it is the first target that needs to be built. 

Also note that there are no backward looking arrows. What if `site_data` relied on `site_data_styled`? In order to satisfy that relationship, an arrow would need to swing back up from `site_data_styled` and connect with `site_data`. Unfortunately, this creates a [cyclical dependency](https://en.wikipedia.org/wiki/Circular_dependency) since changes to one target change the other target and changes to that target feed back and change the original target...so on, and so forth...

This potentially infinite loop is confusing to think about and is also something that dependency managers can't support. If your pipeline contains a cyclical dependency, you will get an error when you try to run `tar_make()` or `tar_visnetwork()` that says "dependency graph contains a cycle". We won't say much more about this issue here, but note that in the early days of building pipelines if you run into the cyclical dependency error, this is what's going on. 

---

:keyboard: Add a comment when you are ready to move on.  

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>
