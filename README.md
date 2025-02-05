# Git Developer Contributions Analysis

This repository contains a command-line tool, written in `python`, to track developers' contributions to one or more Git repositories within a particular time range. GitHub's Insights tools and charts are not extremely useful, and often omit contributors or give misleading statistics.

This script calculates the following, for each developer in each repository:

- number of **merges**
- number of **commits**
- number of **lines added**
- number of **lines deleted**
- number of **files changed**

The results can be formatted as `csv`, `json`, or `markdown`.

## Installation

You need python3.

```
gh extension install gh-cli-for-education/gh-activity
```

## Running the program

1. Fork this repository and clone it to your local machine
1. Grant yourself execute permissions to the `python` script, e.g. `chmod u+x *.py`.

## Usage

The command `gh activity --help` shows the usage instructions:

```
usage: gh activity [-h] (-r REPOSITORY | -rf REPOFILE) [-u USER] [-s START] [-e END] [-x EXCLUSIONS] [-f {csv,json,markdown}] [-v]
                   [-V]

optional arguments:
  -h, --help            show this help message and exit
  -r REPOSITORY, --repository REPOSITORY
                        the public URL of the repository whose logs to parse
  -rf REPOFILE, --repofile REPOFILE
                        the path to simple text file with a list of repository URLs to parse
  -u USER, --user USER  The git username to report. Default is all contributing users
  -s START, --start START
                        Start date in mm/dd/yyyy format
  -e END, --end END     End date in mm/dd/yyyy format
  -x EXCLUSIONS, --exclusions EXCLUSIONS
                        A comma-separated string of files to exclude, e.g. --excusions "foo.zip, *.jpg, *.json"
  -f {csv,json,markdown}, --format {csv,json,markdown}
                        The format in which to output the results
  -v, --verbose         Whether to output debugging info
  -V, --version         show program's version number and exit
```

## Example usage

### Single versus multiple repository analytics

The `-r` and `-rf` flags control whether the script looks at a single repository, or a batch of repositories stored in a simple text file.

Output the contributions of all developers to a single repository:

```bash
gh activity -r https://github.com/bloombar/git-developer-contribution-analysis.git
```

Output the contributions of all developers to a set of repositories stored in a file named `repos.txt` (see [example file](./repos.txt)):

```bash
gh activity -rf repos.txt
```

### Individual contributor versus all contributors

By default, the statistics of all contributors are calculated. The `-u` flag can be used to limit the analysis to just a single contributor by referencing their git username.

Output the contributions of only the contributor named `bloombar` to a single repository:

```bash
gh activity -u bloombar -r https://github.com/bloombar/git-developer-contribution-analysis.git
```

The same, but to a batch of repositories listed in the `repos.txt` file:

```
gh activity -u bloombar -rf repos.txt
```

### Custom date range

By default, contributions from a year ago until today are analyzed. Use the `-s` and `-e` flags to specify a different start and end date, respectively.

Output the contributions to a single repository for a specific date range, inclusive.

```bash
gh activity -s 11/15/2021 -e 12/15/2021 -r https://github.com/bloombar/git-developer-contribution-analysis.git
```

The same, but to a batch of repositories listed in the `repos.txt` file:

```
gh activity -s 11/15/2021 -e 12/15/2021 -rf repos.txt
```

### Formatting the results

The results can be formatted as `csv`, `json`, or a `markdown` table. The default is `csv`. Use the `-f` flag to control the output format.

```
gh activity -s 11/15/2021 -e 12/15/2021 -rf repos.txt -f markdown
```

### Combinations

Flags can be combined to provide more targeted analysis, e.g. a specific contributor over a specific date range

```
gh activity -u bloombar -s 11/15/2021 -e 12/15/2021 -r https://github.com/bloombar/git-developer-contribution-analysis.git -f json
```

The same, but to a batch of repositories listed in the `repos.txt` file:

```
gh activity -u bloombar -s 11/15/2021 -e 12/15/2021 -rf repos.txt -f json
```

## Words of caution

### Large numbers of additions or deletions

If a particular contricutor shows a very large number of additions or deletions, typically on the order of many hundreds or thousands, this could be a sign of poor usage of version control.

Most likely, the contributor has failed to update their version control settings to ignore platform or 3rd party code to (i.e. has not updated their `.gitignore` file prior to adding such code), and is therefore tracking additions/deletions of code that is not theirs. The entire contribution for that user during this date range should be ignored in this case until the developer fixes this problem.

### Redundant usernames

The output of the script sometimes lists the same individual contributor under more than one git username... This is most likely due to different username settings for various git and GitHub clients.

If a single developer has multiple usernames that all show the same statistics, then only count those stats once. Otherwise, if a single developer has multiple usernames that show different statistics, then add them together to come up with the total for that developer.

## Copyright

* [Code developed by Bloombar: bloombar/git-developer-contribution-analysis](https://github.com/bloombar/git-developer-contribution-analysis) 2020-2021
* This is just an adaptation to make it a gh extension. Casiano Rodríguez (2022)