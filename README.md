# Predict future sales

The [data](./Data/) consist of daily item sales of
[1C Company](https://en.wikipedia.org/wiki/1C_Company)'s shops, from
January 2013 to October 2015.

The objective is to predict monthly volumes, i.e. counts, of items
sold in each shop for November 2015.

To help us in this quest, we have information about shop, item and
item category ID's and names, item prices and volumes (sold or returned),
and date information. The test set consists of shop and item ID's.

# Structure of the folder

* All codes are stored under `Code/` in five separate notebooks, namely
    1. first insight into the data, train/test split
    2. exploratory data analysis
    3. data pre-processing
    4. level-1 models producing meta-features for ensembling
    5. level-2 models fitted on the meta-features

* Intermediary results (pre-processed data, meta-features, etc.) are stored
  in `Intermediary results`. Unfortunately, due to the size of those files,
  only the smallest are stored in this repository; others can be reproduced
  using the respective notebooks.

* An empty `Submission` folder where submission files are stored by the last
  two notebooks.

* The file `instructions.md` gives a quick overview of the methodolgy used.
