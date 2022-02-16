#### HOW-TO: generate the dataset

When running any notebook that makes use of data for the first time, it will automatically attempt to download and preprocess the [origin dataset](https://doi.org/10.7910/DVN/CHMUYZ). \
Once done, your project structure should look like this:

```
.
├── notebooks
│   └── ...
├── resources
│   ├── dataset
│   │   ├── raw
│   │   │   ├── newsdata
│   │   │   │   └── ...
│   │   │   ├── labels.tsv
│   │   │   └── raw.tar.bz2
│   │   └── dataset.csv    
│   └── preprocessed
│       ├── dictionary.dict
│       └── proc_dataset.pkl
├── ...
├── README.md
└── requirements.txt
```

Alternatively, one could directly run the `load-dataset.ipynb` notebook inside `./notebooks/data/` to generate the dataset.

The operation is split in two phases:
1. the dataset is generated and stored as a CSV file named `dataset.csv` in `./resources/dataset/`, containing only articles from sources deemed either reliable or unrealiable (articles from sources labeled as mixed or unlabeled are ignored);
2. the dataset is loaded from disk to memory and preprocessed, then subsampled due to its size, afterwhich it is stored as a file named `proc_dataset.pkl` inside `./resources/processed/`, along with other auxiliary resources.

Each of the two phases is managed by its own notebook, respectively `_generate-dataset.ipynb` and `_preprocess-dataset.ipynb` in `./notebooks/data`. \
Neither of those can be launched independently and must be ran from `load-dataset.ipynb`.

The `load-dataset.ipynb` notebook contains parameters that can be tuned to alter the data generation and preprocessing processes. 

As long as the newly generated preprocessed files in `./resources/preprocessed/` exist, any file inside `./resources/dataset/` isn't needed and can therefore be removed.

One can repeat the preprocessing operation by deleting either one of `proc_dataset.pkl` or `dictionary.dict` contained in `./resources/preprocessed/` and rerunning the `load-dataset.ipynb` notebook. \
This operation is flexible as it will automatically download and/or generate any needed resource, though the only one strictly required is `dataset.csv` in `./resources/dataset/`, so one should consider keeping it they intend to repeat the preprocessing using different parameters.

Be warned that generating and preprocessing the dataset will definetely take a while to complete and might even freeze or crash your system in the process if not enough memory is available (or even disk space, although about a dozen GBs should be enough). \
Should that happen during the second phase, the operation can be resumed by rerunning the notebook, with the first part being skipped automatically (this should also require less memory). \
On the flip side -- and as already stated -- this operation is required to be ran just once, afterwhich the `./resources/dataset/` directory can be deleted to save space (again, as long as the preprocessed files exist). \
It is recommended to restart all kernels to free up memory once the preprocessed dataset is generated.
