In general, if building part of a pipeline is "expensive" (i.e., takes more than a trivial amount of time for a computer to execute), it should be a separate target. In the example above :point_up:, expensive sections included fetching data and plotting. 

Additional reasons to create a target include:
- If some element in the pipeline may fail (such as downloading data from the internet), isolating this brittle part of the project as a target with a corresponding function makes it faster to get past. This is because your target focuses on accomplishing _only_ the brittle step, instead of, for example, also attempting to process and plot downloaded data all within the same function.
- Sometimes a target is created in order to make it easier to defer a decision for later. If we have an expensive geoprocessing task but the methods for the final way of summarizing the results is in flux, it might make sense to break this function and target into two functions and two targets: the major parts of the geoprocessing step in one function-target pair, and the smaller summary process in the second. 
- Targets are easy to inspect and dig into (e.g., `my_target <- scmake('my_target')`, or reading in a file that was created). If there is an intermediate step in a workflow that will likely need to be examined, it may deserve a target.
- Lastly, if a configuration or value is shared accross many other targets, the configuration itself might deserve a stand alone target, even if generating that target is computationally cheap. In our water quality data pull example, the `wqp.config` target is an example of a shared configuration. Within that target, there is (among other things) a string that specifies how lake sites are queried in the web service. If that query parameter changes in the future, making the change to the file behind the `wqp.config` target would propagate into the necessary updates to the data pulls run with `get_wqp_data`.

---

But of course there is a cost to creating many targets: you'll end up typing a lot more, a lot of additional files will be created that need to be stored, and the addition of more targets makes it is harder to navitate the `remakefile`.

<hr>
<h3 align="center">Close this issue when you are ready to move on to the next activity</h3>



