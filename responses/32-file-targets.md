### More details on file targets

File targets are very flexible and of course, are also easy to share or store elsewhere. 

Additionally, many targets are either language agnostic (e.g., csv, tsv, txt, nc files) _or_ are meant to be shared across languages, such as the how the [feather file](https://blog.rstudio.com/2016/03/29/feather/) was designed for exchange between R and Python.

When specifying a target in a remakefile recipe with file targets, the path to the file needs to be either absolute or relative to the working directory that the `remake.yml` file is in. 

---

Most of the guidance you'd see on the [remake package](https://github.com/richfitz/remake) whould steer you away from using files as targets, since the benefits of files are quite small compared to the advantages of using objects. In fact, one of the edits I made to the background on target types that was borrowed from `remake` was to remove the statement 
> "With remake though, [file targets] should probably only be the beginning or end points of an analysis", which is referring to the end products of a pipeline likely being figures, tables, markdown files, or documents (all files) and encouraging all other targets to be objects. 

For reasons that will become cleared in the future, we instead recommend that files be used more liberally than objects because of two reasons: 1) ability to store data remotely in file format, and 2) ease of collaboration. You'll here more about this in the intermediate pipelines courses and when you see some more of the team's pipelines in practice. 

---
:keyboard: Activity: Close this issue when you are ready to move on to the next assignment

<hr>
<h3 align="center">I'll sit patiently until this issue is closed.</h3>