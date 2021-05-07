
You are nearly done with the second pipelines course! We have a few final thoughts before you level up your pipelines capability :trophy:

---

First, you may have seen a folder called `_targets` :file_folder: created after running `tar_make()` for the first time. This folder contains the intermediate objects built when running the pipeline, as well as, status metadata about the most recent build for each target. This kind of information is extremely important to _you_ on _your_ computer, but your collaborators will have unique status information on their own computer. For this reason, make sure to add `_targets/*` to your `.gitignore` file so that it won't be committed.

We encourage excessive commenting of code! It is so important to explain what you are doing and why you are doing it, especially when you come up with a really clever line of code :bulb:. With `targets`, any commented lines of code (ones beginning with `#`) you add to your functions will not be seen as a change to the file and will not trigger a rebuild of targets. So comment away freely, regardless of whether it's before or after you've written and run a function!

---

`targets` is part of the rOpenSci community of packages and is being used by a wide net of R users. If you have further questions, you can always consult a Data Science colleague, but also try Google and take advantage of the extensive documentation available in the [`targets` R Package User Manual](https://books.ropensci.org/targets/).

<h3 align="center">Close this issue to continue</h3>
