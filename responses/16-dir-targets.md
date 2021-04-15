### How (not to) depend on a directory for changes

You might have a project where there is a directory :file_folder: with a collection of files. To simplify the example, assume all of the files are `.csv` and have the same format. As part of the hypothetical project goals, these files need to be combined and formatted into a single plottable data.frame. 

In a data pipeline, we'd want assurance that any time the number of files changes, we'd rebuild the resulting data.frame. Likewise, if at any point the _contents_ of any one of the files changes, we'd also want to re-build the data.frame. 

This hypothetical example could be coded as 
```yaml
sources:
  - combine_files.R

targets:
  all:
    depends: figure_1.png

  plot_data:
    command: combine_into_df('1_fetch/in/file1.csv','1_fetch/in/file2.csv','1_fetch/in/file3.csv')
    
  figure_1.png:
    command: my_plot(plot_data)
```

:point_up: This coding would work, as it tells the dependency manager which files to track for changes, and where the files in the directory are. But this solution is less than ideal, both because it doesn't scale well to **many** files, and because it doesn't adapt to new files coming into the `1_fetch/in` directory :file_folder: (the pipeline coder needs to manually add file names)

---

Alternatively, what about adding the directory as an input to the recipe, like this (you'd also need to modify your `combine_into_df` function to use `dir(work_dir)` to generate the file names)?
```yaml
sources:
  - combine_files.R

targets:
  all:
    depends: figure_1.png

  plot_data:
    command: combine_into_df(work_dir = '1_fetch/in')
    
  figure_1.png:
    command: my_plot(plot_data)
```

After running `scmake()`, :point_up: this looks like it works, but results in a rather cryptic warning message might end up being ignored:
```
Warning messages:
1: In structure(.Call(C_Rmd5, files), names = files) :
  md5 failed on file '1_fetch/in'
```

An explaination of that warning message - In order to determine if file contents have changed, remake uses `md5sum()` from the `tools` package to create a unique hash for each target. You can take a look at what this does:
```
tools::md5sum('3_visualize/out/figure_1.png')
      3_visualize/out/figure_1.png 
"500a71be79d1e45457cdb2c31a03be46" 
```
`md5sum()` fails when you point it toward a directory, since it doesn't know what to do with that kind of input. 

---

A _third_ strategy might be to create a target that lists the contents of the directory, and then uses that list as an input:
```yaml
sources:
  - combine_files.R

targets:
  all:
    depends: figure_1.png
  
  work_files:
    command: dir('1_fetch/in')
  
  plot_data:
    command: combine_into_df(work_files)
    
  figure_1.png:
    command: my_plot(plot_data)
```
:point_up: This approach is close, but has a few flaws: 1) because '1_fetch/in' can't be tracked as a real target, changes within that directory aren't tracked (same issue as the previous example), and 2) if the _contents_ of the files change, but the number and names of the files _don't_ change, the `work_files` target won't appear changed, and therefore the necessary rebuild of `plot_data` would be skipped (another example of "skip the work you don't need" in action)

Instead, we would avocate for a modified approach that combines the work of `md5sum` with the collection of files returned with `dir`. If instead of `work_files` being a vector of file names (in this case, `c('1_fetch/in/file1.csv','1_fetch/in/file2.csv','1_fetch/in/file3.csv')`) - can we make it a data.frame of file names and their associated hashes?
```
# A tibble: 3 x 2
  filepath             hash                            
  <chr>                <chr>                           
1 1_fetch/in/file1.csv 530886e2b604d6d32df9ce2785c39672
2 1_fetch/in/file2.csv dc36b6dea28abc177b719015a18bccde
3 1_fetch/in/file3.csv c04f87fb9af74c4e2e2e77eed4ec80f3
```
Now, if the `combine_into_df` can by modified to accept this type of input (i.e., simply grab the filepath with `$filepath`), _then_ we'll have an input that both reflects the total number of files in the directory, and captures a reference hash that will reveal any future changes. Yay! :star2: This works because a change to any one of those hashes will result in a change in the hash of the new modified `work_files` data.frame, which then would cause a rebuild of `plot_data`. 

But unfortunately, because `work_files` still relies on a directory and therefore can't be tracked, we're still missing a critical part of the solution. In the next topic, we'll reveal a more elegant way to trigger rebuilds, but for now, note you'd need to manually run

`scmake('work_files', force = TRUE)` to force scipiper to dig into that directory and hash all of the files. If the result of `work_files` that comes back is the same as before, running `scmake()` won't build anything new and we can be confident that `figure_1.png` is up to date. 

---

:keyboard: Add a comment when you are ready to move on.  

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>
