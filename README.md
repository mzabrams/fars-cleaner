![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/mzabrams/fars-cleaner)
![PyPI](https://img.shields.io/pypi/v/fars-cleaner)
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)
[![DOI](https://zenodo.org/badge/252038452.svg)](https://zenodo.org/badge/latestdoi/252038452)

# FARS Cleaner `fars-cleaner`

`fars-cleaner` is a Python library for downloading and pre-processing data 
from the Fatality Analysis Reporting System, collected annually by NHTSA since
 1975 (). 

## Installation

The preferred installation method is through `conda`.
```bash
conda install -c conda-forge fars-cleaner
```
You can also install with [pip](https://pip.pypa.io/en/stable/).

```bash
pip install foobar
```

## Usage

### Downloading FARS data
The `FARSFetcher` class provides an interface to download and unzip selected years from the NHTSA FARS FTP server. 
The class uses `pooch` to download and unzip the selected files. By default, files are unzipped to your OS's cache directory.

```python
from fars_cleaner import FARSFetcher

# Prepare for FARS file download, using the OS cache directory. 
fetcher = FARSFetcher()
```
Suggested usage is to download files to a data directory in your current project directory. 
Passing `project_dir` will download files to `project_dir/data/fars` by default. This behavior can be 
overridden by setting `cache_path` as well. Setting `cache_path` alone provides a direct path to the directory
you want to download files into.
```python
from pathlib import Path
from fars_cleaner import FARSFetcher

SOME_PATH = Path("/YOUR/PROJECT/PATH") 
# Prepare to download to /YOUR/PROJECT/PATH/data/fars
# This is the recommended usage.
fetcher = FARSFetcher(project_dir=SOME_PATH)

# Prepare to download to /YOUR/PROJECT/PATH/fars
cache_path = "fars"
fetcher = FARSFetcher(project_dir=SOME_PATH, cache_path=cache_path)

cache_path = Path("/SOME/TARGET/DIRECTORY")
# Prepare to download directly to a specific directory.
fetcher = FARSFetcher(cache_path=cache_path)
```

Files can be downloaded in their entirety (data from 1975-2018), as a single year, or across a specified year range.
Downloading all of the data can be quite time consuming. The download will simultaneously unzip the folders, and delete 
the zip files. Each zipped file will be unzipped and saved in a folder `{YEAR}.unzip`
```python
# Fetch all data
fetcher.fetch_all()

# Fetch a single year
fetcher.fetch_single(1984)

# Fetch data in a year range (inclusive).
fetcher.fetch_subset(1999, 2007)
```

### Processing FARS data
```python
import fars_cleaner

#foobar.pluralize('word') # returns 'words'
#foobar.pluralize('goose') # returns 'geese'
#foobar.singularize('phenomena') # returns 'phenomenon'
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License
[BSD-3 Clause](https://choosealicense.com/licenses/bsd-3-clause/)