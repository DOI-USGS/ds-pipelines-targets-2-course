**Welcome to the second installment of "introduction to data pipelines"** at USGS, @{{ user.username }}!! :sparkles:

We're assuming you were able to navigate through the [intro-to-pipelines](https://lab.github.com/USGS-R/intro-to-pipelines) course and that you learned a few things about organizing your code for readability, re-use, and collaboration. You were also introduced to two key things through the makefile: a way to program connections between functions and files, and the concept of a dependency manager that skips parts of the workflow that don't need to be re-run. 

---
### Recap of pipelines I

First, a recap of key concepts that came from [intro-to-pipelines](https://lab.github.com/USGS-R/intro-to-pipelines) :point_down:
- Data science work should be organized thoughtfully. As Jenny Bryan notes, "File organization and naming are powerful weapons against chaos".
- Capture all of the critical phases of project work with descriptive directories and function names, including how you "got" the data.
- Turn your scripts into a collection of functions, and modify your thinking to connect deliberate outputs from these functions ("targets") to generate your final product.
- "Skip the work you don't need" by taking advantage of a dependency manager. There was a video that covered a bit of `make`, and you were asked to experiment with `targets`.
- Invest in _efficient_ reproducibility to scale up projects with confidence. 

This last concept was not addressed directly but we hope that the small exercise of seeing rebuilds in action got you thinking about projects that might have much more lengthly steps (e.g., several downloads or geo-processing tasks that take hours instead of seconds).

### What's ahead in pipelines II

In this training, the focus will be on conventions and best practices for making better, smarter pipelines for USGS Data Science projects. You'll learn new things here that will help you refine your knowledge from the first class and put it into practice. Let's get started!


:keyboard: Activity: Add collaborators and close this issue to get started.

As with pipelines I, please invite a few collaborators to your repository so they can easily comment and review in the future. In the :gear: Settings widget at the top of your repo, select "Manage access" (or use [this shortcut link]({{ repoUrl }}/settings/access)). Go ahead and invite aappling-usgs and jread-usgs. It should look something like this: 
![add some friends](https://user-images.githubusercontent.com/2349007/81471981-c0094900-91ba-11ea-93b0-0ffd31ec4ea9.png)

:bulb: Tip: Throughout this course, I, the Learning Lab Bot, will reply and direct you to the next step each time you complete an activity. But sometimes I'm _too_ fast when I :hourglass_flowing_sand: give you a reply, and occasionally you'll need to refresh the current GitHub page to see it. Please be patient, and let my humans ([Alison](https://github.com/aappling-usgs) or [Jordan](https://github.com/jread-usgs)) know if I seem to have become completely stuck.

<hr>
<h3 align="center">I'll sit patiently until you've closed the issue.</h3>
