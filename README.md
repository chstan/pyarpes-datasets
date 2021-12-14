# What is this?

This is a companion repository to [PyARPES](https://github.com/chstan/arpes). It provides data which new users can use to get up to speed in how ARPES data can be analyzed.

PyARPES used to include datasets with the code by default. This bloats the packages hosted on [pip](https://pypi.org/project/arpes) and [conda](https://anaconda.org/arpes/arpes). While good for users, this approach prevented adding more dataset and in the limit of more and heavier files abuses the trust package repositories place in package authors.

With this repository, PyARPES moves to a linked data approach. Access to data still works in PyARPES as normal, but instead of packaging data directly, PyARPES caches data downloaded from this repository on a client's computer.

# Adding a new dataset

There are two things you need to do to add a dataset here. 

## Creating the dataset

First, you need to create the data itself. You should make sure it loads in PyARPES using some loader. Remember the `location=` key which is required as this will be needed later.

Consider stripping out any proprietary information or personally identifying information like:

1. Experimenter names
2. Times of experiments and facilities used
3. Types of hardware used and any non-public metadata

To preserve data confidentiality while making some test data in a given format available, you can consider smoothing or binning the data. Binning may be helpful on the grounds that it reduces file sizes too.

Once you have your data, commit it in the `data` folder here. If your dataset requires several files, for instance like the formats produced at ALS beamline 4.0.2 (MERLIN), include all files inside a single directory. 

Your data should be located at 

```
data/{dataset-name}.{extension}
```

or at 

```
data/{dataset-name}/{first-filename}.{extension}
...
data/{dataset-name}/{last-filename}.{extension}
```

This is important so that the PyARPES downloader can fetch the data.

## Registering the data with PyARPES

Because data is no longer stored in place with the repository you will have to tell PyARPES to go look for the example. In practice, a manifest hosted here could be used, but the current method makes it straightforward to specify versions: just refer to the version appropriate piece of data from PyARPES.

You need to modify the contents of `arpes/io.py` in order to build this association. Have a look at the definition of `ExampleData` and `DATA_EXAMPLES` in that file for a model of adding a data source.

In principle, you need only add an entry to `DATA_EXAMPLES` and a method for loading the data on `ExapleData`.