# Fake News Detection

A group of interactive notebooks built with the intent of studying the process of automated fake news detection using machine learning.



#### HOW-TO: build the dataset

The first time any notebook that makes use of data is executed, the [source dataset](https://doi.org/10.7910/DVN/RBKVBM) will automatically be downloaded and processed.
Once done, the project structure should look somewhat like this:

```
.
├── notebooks
│   └── ...
├── resources
│   ├── source
│   │   ├── raw
│   │   │   ├── newsdata
│   │   │   │   └── ...
│   │   │   ├── labels.tsv
│   │   │   └── raw.tar
│   │   └── source_dataset.csv    
│   └── processed
│       ├── dictionary.dict
│       └── dataset.pkl
└── ...
```

Alternatively, one could directly run the `load-dataset.ipynb` notebook inside `./notebooks/data/`.

The operation is split in two phases:
1. the dataset is generated and stored as a CSV file named `source_dataset.csv` in `./resources/source/`, containing only articles from sources deemed either reliable or unrealiable (articles from sources labeled as mixed or unlabeled are dropped);
2. the dataset is loaded from disk to memory and preprocessed, then subsampled due to its size, afterwhich it is stored as a file named `dataset.pkl` inside `./resources/processed/`, along with other auxiliary resources.

Each of the two phases is managed by its own notebook, respectively `_generate-dataset.ipynb` and `_preprocess-dataset.ipynb` in `./notebooks/data`.
Neither of those two can be launched independently.

The `_preprocess-dataset.ipynb` and `_meta.ipynb` notebooks contain parameters that can be tuned to alter the data generation and preprocessing phases. 

As long as the newly generated preprocessed files in `./resources/processed/` exist, any file inside `./resources/source/` isn't needed and can therefore be removed.

One can repeat the preprocessing operation by deleting either one of `dataset.pkl` or `dictionary.dict` contained in `./resources/processed/` and rerunning the `load-dataset.ipynb` notebook. \
This operation is flexible as it will automatically download and/or generate any needed resource, though the only one strictly required is `source_dataset.csv` in `./resources/source/`, so one should consider keeping it they intend to repeat the preprocessing using different parameters.

Be advised that generating and preprocessing the dataset will definetely take a while to complete and might even freeze or crash your system in the process if not enough memory is available (or even disk space, although about a dozen GBs should be enough). \
Should that happen during the second phase, the operation can be resumed by rerunning the notebook, with the first part being skipped automatically (this should also require less memory). \
On the flip side -- and as already stated -- this operation is required to be ran just once, afterwhich the `./resources/source/` directory can be deleted to save space (again, as long as the preprocessed files exist). \
It is recommended to restart all kernels to free up memory once the processed dataset is generated in order to free some memory.
