### The `target_name` special variable. 

#### Simplifying `target:command` relationships and reducing duplication

In your repo, there is a remake files that specifies relationships between targets, commands, and other targets. 

For one of the targets, you'll see this:

```yaml
  1_fetch/out/site_info.csv:
    command: nwis_site_info(fileout = '1_fetch/out/site_info.csv', site_data)
```

It seems odd to duplicate the _name_ of the target twice. First it is used to declare the file target path as `1_fetch/out/site_info.csv`. But then it is used a second time because the `nwis_site_info` needs to know the name of the file to write the downloaded data too. Doesn't this seem duplicative and potentially dangerous (what if you make a typo to the `fileout` argument and it writes to a different file)?

The `target_name` variable is a useful way to avoid the duplication and danger of repeating the name of the target as one of the inputs. We can instead write the target recipe as 

```yaml
  1_fetch/out/site_info.csv:
    command: nwis_site_info(fileout = target_name, site_data)
```

Here, `target_name` will tell scipiper to use "1_fetch/out/site_info.csv" for the `fileout` argument in the `nwis_site_info` function when building the target. Using `target_name` not only saves us some time and makes the construction of pipelines less error prone, it also helps us create patterns that can be used to generate multiple targets in a simpler fashion. 

Imagine part of the target name is used to specify something within the function call, such as a site number. 

```yaml
  1_fetch/out/nwis_01427207_data.csv:
    command: download_nwis_site_data(target_name)

  1_fetch/out/nwis_01432160_data.csv:
    command: download_nwis_site_data(target_name)
    
  1_fetch/out/nwis_01436690_data.csv:
    command: download_nwis_site_data(target_name) 

```
the `download_nwis_site_data` function uses a regular expression to extract the site number from the input file name in order to make a web service request for data. Here, even though it seems goofy to create so many download targets, we reduce the places mistakes can be made by using `target_name` instead of repeating the use of the file name.

---

:keyboard: Add a comment when you are ready to move on.  

<hr>
<h3 align="center">I'll sit patiently until you comment</h3>