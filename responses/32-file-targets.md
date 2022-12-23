### More details on file targets

File targets are very flexible and, of course, are also easy to share or store elsewhere. 

Additionally, many file formats are either language agnostic (e.g., csv, tsv, txt, nc files) _or_ are meant to be shared across languages, such as the [feather format](https://blog.rstudio.com/2016/03/29/feather/) designed for exchange between R and Python.

When specifying a file target in a makefile, the path to the file needs to be either absolute or relative to the working directory that the `_targets.R` file is in. 

---

Since file targets in the `targets` package are not the default and require you to add `format = "file"`, you may feel deterred from using files as targets. It's true, the benefits of files are often small compared to the advantages of using objects. However, we still recommend that files be used liberally, especially for targets that you'll want to access outside of R (e.g., browsing figure files in Finder/Windows Explorer; opening a spatial data file in a GIS) or share with others (e.g., using outputs from one pipeline as inputs to another). 

---
:keyboard: Activity: Close this issue when you are ready to move on to the next assignment

<hr>
<h3 align="center">I'll sit patiently until this issue is closed.</h3>
