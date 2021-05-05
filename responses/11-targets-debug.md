### How to inspect parts of the pipeline and variables within functions

If you've written your own functions or scripts before, you may have run into the red breakpoint dot :red_circle: on the left side oyour script window:

![breakpoint](https://support.rstudio.com/hc/en-us/article_attachments/201608458/editor-breakpoint.png)

Breakpoints allow you to run a function (or script) up until the line of the breakpoint, and then the evaluation pauses. You are able to inspect all variables available at that point in the evaluation, and even step carefully forward one line at a time. It is out of scope of this exercise to go through exactly how to use debuggers, but they are powerful and helpful tools. It would be a good idea to [read up on them](https://support.rstudio.com/hc/en-us/articles/205612627-Debugging-with-RStudio) if you haven't run into breakpoints yet. 

In `targets`, you can't set a breakpoint in the "normal" way, which would be clicking on the line number after you sourced the script. Instead, you need to use the other method for debugging in R, which requires adding the function call `browser()` to the line where you'd like the function call to stop and specifying an additional argument when you call `tar_make()`. 

:warning: Check your RStudio version (go to the Help menu and click About RStudio). If you have a version earlier than v1.3.5, you may want to consider [updating RStudio](https://www.rstudio.com/products/rstudio/download/#download) before proceeding to have the smoothest experience in debugging mode. :warning:

---

You have a working, albeit brittle, pipeline in your course repository. You can try it out with `targets::tar_make()`. This pipeline has a number of things you'll work to fix later, but for now, it is a useful reference. The pipeline contains a _targets.R file and several functions defined in `.R` files. 

So, if you wanted to look at what `download_files` were created within the `download_nwis_data()` function, you could set a breakpoint by adding `browser()` to the `"1_fetch/src/get_nwis_data.R"` file (make sure to hit save for changes to take affect!). Hint: to quickly navigate to this function source code from your makefile, you can put your cursor on the name of the function then click F2 and it will take you to the correct location in the corresponding source file!

![browser()](https://user-images.githubusercontent.com/2349007/82158816-bed9bb00-984f-11ea-8892-b2aeb5e4818d.png)

There is one more step to get your breakpoint to work in `targets`. You will need to add `callr_function = NULL` to your `tar_make()` call. When you run `tar_make(callr_function = NULL)`, you will land right in the middle of line 8. Give it a try on your own.

To navigate while in browser mode, you can use the buttons at the top of your console pane:

![debugnav](https://support.rstudio.com/hc/en-us/article_attachments/201594747/console-toolbar.png)

---

:keyboard: Place a `browser()` in the `for` loop of the `download_nwis_data()` function. Build the pipeline and compare the size of `data_out` through each iteration of the loop using the debugger navigational features. When you are done, don't forget to remove the `browser()` command from that function and then save the R script. Then, comment here on where you think you might find `browser()` handy in future pipelines. 

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>
