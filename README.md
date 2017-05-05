# s3r
Life is short, you shouldn't type so much when interacting with s3 buckets

### Requirements
To use this package you need to have [aws.cli](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) installed and configured on your machine. If you are interacting with a private bucket this'll include configuring your credentials. Default credentials (without specifying a --profile) will be used unless a profile name is specified.

> big caveat, I'm only be testing this package initially on amazon linux and ubuntu, so usage on windows or osx at your own risk.

```
bash> aws configure help
bash> aws s3 configure -profile cred1
```
### Vignette
Just sketching out how I'd like this package to function. This is before I've written any code so don't take this as evidence something should work yet ;)

Formal or simplified environment designation. The **e** environment serves a crucial part in this package, storing you main bucket name, profile, local file path to serve as a cache, default aws arguments. The biggest improvement I hope this package will make on the typical operations with s3 paths is the designation of a working directory at e$wd. This will allow you to list, move, copy, get, or put without using a fully qualified s3 name.
```
R> s3_set_env(bucket = "s3://my-bucket",
          profile = 'cred1',
            cache = /tmp/local,
          	  sse = T,
               wd = "top/next")

R> s3_set_env(bucket = "my-bucket")
```

The basic function to start will include file listing, moving and copying..both within s3 buckets and to local directories
```
R> s3_ls()
# "next/", "table1.txt", "table2.txt", "spreadsheet.xlsx"

R> s3_ls(list.names = F)
#                            PRE next/
# 2017-04-24 19:47:52      12391 table1.txt
# 2017-04-24 19:48:06      90791 table2.txt
# 2017-04-24 19:48:02     728328 spreadsheet.xlsx

R> s3_ls(files.only = T)
# "table1.txt", "table2.txt", "spreadsheet.xlsx"

R> s3_ls(dir.only = T)
# "next/"

R> s3_ls(pattern = "xlsx$")
# "spreadsheet.xlsx"

R> s3_ls(recursive = T)
# "spreadsheet.xlsx", "next/table_in_subfolder.txt"

R> s3_ls(recursive = T, pattern = "table")
# "table1.txt", "table2.txt", "next/table_in_subfolder.txt"
```
