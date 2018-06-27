# ipynb_stripout

Output cells make it difficult to manage iPython notebooks under Git. Diffs become noisy and merge conflicts become common. Certain Jupyter extensions can cause problems too, either by modifying cells whenever the notebook is run or, in some cases, by embedding runtime data which might lead to information leakage.  The `ipynb_stripout` script acts as a Git filter, which can help alleviate some of these problems and allow you to commit a "clean" notebook into version control.

## Previous Work
This repo is forked from [jond3k's version](https://github.com/jond3k/ipynb_stripout), which is itself based on work from [cfriedline](https://github.com/cfriedline/ipynb_template) and [minrk](https://gist.github.com/minrk/6176788). I've added support for stripping some additional types of cells:

* __ExecuteTime__ cells placed by the _ExecuteTime_ extension
* __variables__ cells placed by the _Python Markdown_ extension

I'll add more in the future, as needed. __Pull requests to stip additional cell types are always welcome!__

Another difference with this fork is that I've remove support for older v3 notebook files.  All recent versions of the notebook generate v4 files by default, so this shouldn't have much of an impact.

## Install (OS X)

First you need to install the `ipynb_stripout` script. Run the following from your terminal:

	wget -O /usr/local/bin/ipynb_stripout "https://raw.githubusercontent.com/DavidJBianco/ipynb_stripout/master/ipynb_stripout"
	chmod +x /usr/local/bin/ipynb_stripout

## Configure git

To make it optionally available to all your git repositories:

	git config --global filter.ipynb_stripout.clean ipynb_stripout
	git config --global filter.ipynb_stripout.smudge cat
	git config --global filter.ipynb_stripout.required


## Add to repository

Go to the root of the repository you want to add it to and  `.gitattributes` with the following line:

	echo "*.ipynb filter=ipynb_stripout" >> .gitattributes

Now you can commit the attributes.

	git add .gitattributes
	git commit -m "Added ipynb_stripout. See https://github.com/DavidJBianco/ipynb_stripout" .gitattributes

## Add notebook files to version control

Now you can add your notebook files to git:

    git add test.ipynb

Note that git appears to only configure the filter for a file when it is added to the repo.  If notebooks already exist, you may have to remove and re-add them in order for the filter configuration to become effective for those files.  
