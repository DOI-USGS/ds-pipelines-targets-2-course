
:keyboard: Activity: Make modifications to the working, but less than ideal, pipeline that exists within your course repository

Within the course repo you should see only a `remake.yml` and directories with code or placeholder files for each phase. You should be able to run `scmake()` and build the pipeline, although it may take numerous tries, since some parts of this new workflow are brittle. Some hints to get you started: the `site_data` target is too big, and you should consider splitting it into a target for each site, perhaps using the `download_nwis_site_data()` function directly to write a file. Several of the `site_data_` targets are too small and it might make sense to combine them. Lastly, if it makes sense to use `target_name`, try using that in the `"remake.yml"` file too to simplify the formatting. 

---

When you are happy with your newer, better workflow, create a pull request with your changes and assign Jordan or Alison as reviewers. Add a comment to your own PR with thoughts on how you approached the task, as well as key decisions you made. See details below for some reminders of how to get started working with code and files that exist within the course repsository:

---

Open a git bash shell (Windows:diamond_shape_with_a_dot_inside:) or a terminal window (Mac:green_apple:) and change (`cd`) into the directory you work in for projects in R (for me, this is `~/Documents/R`). There, clone the repository and set your working directory to the new project folder that was created:
```
git clone git@github.com:{{ user.username }}/{{ repo }}.git
cd ds-pipelines-2
```

Now you should create a local branch called "targets" and push that branch up to the "remote" location (which is the github host of your repository). We're naming this branch "targets" to represent concepts in this section of the lab. In the future you'll probably choose branch names according to the type of work they contain - for example, `"pull-oxygen-data"` or `"fix-issue-17"`.

```
git checkout -b targets
git push -u origin targets
```

<hr>
<h3 align="center">A human will interact with your pull request once you assign them as a reviewer</h3>
