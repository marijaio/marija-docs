# Contributor's guide


``Marija`` community welcomes your contribution. To make the process as seamless as possible, we recommend you read this contribution guide.

## Workflow

### Open an issue (optional)
For larger changes, like adding a new article, it's recommended to first
[open an issue](https://github.com/marijaio/marija-docs/issues/new)
regarding the changes you are considering. This is useful because possibly
someone else is already making similar changes.

### Setup your Marija Docs Github Repository
Fork the Marija Docs GitHub repository, make changes in a branch and then send a pull request. We encourage pull requests to discuss changes. Here are the steps in details:

Fork [Marija upstream](https://github.com/marijaio/marija-docs) source repository to your own personal repository. Copy the URL of your Marija Docs fork (you will need it for the `git clone` command below).

```sh
$ git clone <paste saved URL for personal forked marija docs repo>
$ cd marija
```

### Set up git remote as ``upstream``
```sh
$ cd marija-docs
$ git remote add upstream https://github.com/marijaio/marija-docs
$ git fetch upstream
$ git merge upstream/master
...
```

### Create your feature branch
Before making changes, make sure you create a separate branch for these changes:

```
$ git checkout -b my-new-article
```

### Review your changes locally
After you make your changes, try them locally on your computer by running
`jekyll serve`. Learn how to use jekyll on [jekyllrb.com](https://jekyllrb.com).

### Commit changes
After verification, commit your changes. This is a [great post](https://chris.beams.io/posts/git-commit/) on how to write useful commit messages.

```
$ git commit -am 'Add some article'
```

### Push to the branch
Push your locally committed changes to the remote origin (your fork):
```
$ git push origin my-new-article
```

### Create a Pull Request
Pull requests can be created via GitHub. Refer to [this document](https://help.github.com/articles/creating-a-pull-request/) for detailed steps on how to create a pull request. After a Pull Request gets peer reviewed and approved, it will be merged.