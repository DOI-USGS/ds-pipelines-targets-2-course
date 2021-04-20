### More details on file targets

File targets are very flexible and of course, are also easy to share or store elsewhere. 

Additionally, many file targets are either language agnostic (e.g., csv, tsv, txt, nc files) _or_ are meant to be shared across languages, such as the how the [feather file](https://blog.rstudio.com/2016/03/29/feather/) was designed for exchange between R and Python.

When specifying a target in a makefile recipe with file targets, the path to the file needs to be either absolute or relative to the working directory that the `_targets.R` file is in. 

---

Since file targets in the `targets` package are not the default and require you to add `format = "file"`, you may feel deterred from using files as targets. It's true, the benefits of files are quite small compared to the advantages of using objects. For reasons that will become clearer in the future, we recommend that files be used more liberally than objects because of two reasons: 1) ability to store data remotely in file format, and 2) ease of collaboration. You'll here more about this in the next pipelines courses and when you see some more of the team's pipelines in practice. 

---
:keyboard: Activity: Close this issue when you are ready to move on to the next assignment

<hr>
<h3 align="center">I'll sit patiently until this issue is closed.</h3>