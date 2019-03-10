Conda Buildpack
===============

This is a [Heroku Buildpack](https://devcenter.heroku.com/articles/buildpacks) for [Conda](http://conda.pydata.org/), the Python distribution for scientific computing by Continuum Analytics.

This buildpack supports Python3.

This buildpack enables the installation of binary packages through the
open source [conda](http://conda.pydata.org/) application.  Conda is
recognized as being core to Continuum's Anaconda Scientific Python distro
but it's also at the heart of the lighter weight
[Miniconda](http://conda.pydata.org/miniconda.html) distro which we use
here to install _only_ the binary packages we need for our apps deployed
on Heroku.

To control what binary packages are installed by conda, supply a
`conda-requirements.txt` file (which can be created by capturing the output
of `conda list -e` for your working conda environment).
Like when using the standard buildpack for python from Heroku, you can also
still supply a `requirements.txt` file for [pip](https://github.com/pypa/pip)
to process.  In this way, you can install binary packages via conda for
everything you can and still use pip for anything you can't.
Alternatively, you may supply an `environment.yml` file to consolidate `conda` 
and `pip` dependencies into a single file AND specify the addition of 
condo channels:

`environment.yml`:

```yml
name: openmc-dash-interface
channels:
    - conda-forge
dependencies:
- python=3.6
- anaconda
- pip
- pip:
  - dash
  - numpy
  - pandas
  - gunicorn
```



Usage
-----

You can add it to upcoming builds of an existing application:

```console
$ heroku buildpacks:set https://github.com/SterlingButters/conda-buildpack.git -a [app-name]
```


## Warning

Heroku limits the final application footprint (slug) size to 500MB (as of May 22, 2017). Start small. In case the slug size limit is exceeded, deleting the build cache through the [heroku-repo plugin](https://github.com/heroku/heroku-repo#purge_cache) might help.

## Tip
This buildpack will result in a large slug size upon first deployment to heroku app (~300MB). Resulting builds will be much smaller (~100MB). One solution to the slug size problem is adding a large (even if it's critical) directory in .slugignore, pushing the app to heroku, then removing the directory from git ignore, then pushing again. 


