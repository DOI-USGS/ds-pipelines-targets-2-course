
You should now have a working pipeline that can run with `tar_make()`. Your current pipeline likely only has one file target, which is the final plot. 

We want you to get used to exchanging objects for files and vice versa, in order to expose some of the important differences that show up in the makefile and also in the way the functions are put together. 


:keyboard: Activity: Open a PR where you swap at least two file targets to be object targets, and change one object target to be a file target. Run `tar_make` and open a pull request. Paste your build status as a comment to the PR and assign your designated course instructor as a reviewer. 

You should create a new local branch on which to save these changes. Let's call it "swap-targets" to capture the goals of this activity.

```
git checkout -b swap-targets
git push -u origin swap-targets
```

<hr>
<h3 align="center">I'll sit patiently until your pull request has been merged</h3>

