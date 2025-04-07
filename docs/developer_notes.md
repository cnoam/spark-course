# Notes for developer/contributor of the course

# How to add/modify a notebook to this repo?
You need to create a [pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)


# How to prevent dummy changes in the notebook
After running a notebook and saving it, the file contents (*.ipynb) is changed even if the output has not be modified.
This makes finding real modification difficult, so I built a git hook that will not allow committing notebooks that already run.

This means that before commiting, "clear all outputs" in the notebook and save it.

To enable the hook:

```
cd .git/hooks
cp pre-commit.sample pre-commit
chmod +x pre-commit
```
patch pre-commit  check_notebooks.patch

-------
the patch:

```
cat check_notebooks.patch
1c1
< #!/bin/sh
---
> #!/bin/bash
46a47,63
>
>
> # Noamc{
> changedFiles=`git diff-index  --cached --name-only $against`
> for fname in $changedFiles
> do
>  	# check *.ipynb files
> 	if [[ "$fname" == *ipynb ]]
> 	then
> 		grep -E '"execution_count": [[:digit:]]+' "$fname" >/dev/null
> 		if [ $? -eq 0 ]; then
> 		echo  "Please clear all output from the notebook file \"$fname\" before commiting."
> 		exit 1
> 		fi
> 	fi
> done
> # Noamc}
```


# Supplying dependency JARs
Instead of relying on downloading from Maven as in
`config('spark.jars.packages', 'org.apache.spark:spark-sql-kafka-0-10_2.12:3.2.0')`

I found the list of dependencies, downloaded them and then removed the largest files based on trial and error.

I took the package name, lookd at Maven how the \<dependecy\> looks and created a pom.xml file using a simple example.

Then ran `mvn dependency:copy-dependencies`
