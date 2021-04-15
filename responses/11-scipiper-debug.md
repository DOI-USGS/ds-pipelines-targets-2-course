### How to inspect parts of the pipeline and variables within functions

If you've written your own functions or scripts before, you may have run into the red breakpoint dot :red_circle: on the left side oyour script window:

![breakpoint](https://support.rstudio.com/hc/en-us/article_attachments/201608458/editor-breakpoint.png)

Breakpoints allow you to run a function (or script) up until the line of the breakpoint, and then the evaluation pauses. You are able to inspect all variables available at that point in the evaluation, and even step carefully forward one line at a time. It is out of scope of this exercise to go through exactly how to use debuggers, but they are powerful and helpful tools. It would be a good idea to read up on them if you haven't run into breakpoints yet. 

In `scipiper`, you can't set a breakpoint in the "normal" way, which would be clicking on the line number after you sourced the script. Instead, you need to use the other method for debugging in R, which requires adding the function call `browser()` to the line where you'd like the function call to stop. 

---

You have a working, albeit brittle, pipeline in your course repository. You can try it out with `scipiper::scmake()`. This pipeline has a number of things you'll work to fix later, but for now, it is a useful reference. The pipeline contains several functions which are defined in `.R` files. 

So, if you wanted to look at what `download_files` where created within the `download_nwis_data` function, you could set a breakpoint by adding `browser()` to the `"1_fetch/src/get_nwis_data.R"` file:

![browser()](https://user-images.githubusercontent.com/2349007/82158816-bed9bb00-984f-11ea-8892-b2aeb5e4818d.png)

Then, running `scmake()` will land you right in the middle of line 8. Give it a try on your own.

---

:keyboard: comment on where you think you might find `browser()` handy in future pipelines. 

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>
