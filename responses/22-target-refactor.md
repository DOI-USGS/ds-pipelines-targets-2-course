
:keyboard: Activity: Make modifications to the working, but less than ideal, pipeline that exists within your course repository

Within the course repo you should see only a `_targets.R` and directories with code or placeholder files for each phase. You should be able to run `tar_make()` and build the pipeline, although it may take numerous tries, since some parts of this new workflow are brittle. Some hints to get you started: the `site_data` target is too big, and you should consider splitting it into a target for each site, perhaps using the `download_nwis_site_data()` function directly to write a file. Several of the `site_data_` targets are too small and it might make sense to combine them.

---

When you are happy with your newer, better workflow, create a pull request with your changes and assign Jordan or Alison as reviewers. Add a comment to your own PR with thoughts on how you approached the task, as well as key decisions you made. 

You should create a local branch called "refactor-targets" and push that branch up to the "remote" location (which is the github host of your repository). We're naming this branch "refactor-targets" to represent concepts in this section of the lab. In the future you'll probably choose branch names according to the type of work they contain - for example, `"pull-oxygen-data"` or `"fix-issue-17"`.

```
git checkout -b refactor-targets
git push -u origin refactor-targets
```

<hr>
<h3 align="center">A human will interact with your pull request once you assign them as a reviewer</h3>
