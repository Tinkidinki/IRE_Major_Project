# Downloaded Data


The following datasets need to be downloaded and placed into this directory.

To download from the command line, use the instructions at the end of this page.

## Google News

Download and extract the pre-trained word vector dataset based on [Google News](https://drive.google.com/file/d/0B7XkCwpI5KDYNlNUTTlSS21pQmM/edit?usp=sharing) (this is a large file of approximately 1.5Gb).

Further details on this dataset may be found on the [word2vec webpage](https://code.google.com/archive/p/word2vec/).

## SemEval-2019 Datasets

The following are links to the dataset after the Twitter data of 2019 has been converted to the 2017 file format. Please follow the below instructions to download.

## To download via the command line

### Download

```
cd <your-branchLSTM-directory>/downloaded_data
wget https://s3.amazonaws.com/dl4j-distribution/GoogleNews-vectors-negative300.bin.gz
pip install gdown
gdown https://drive.google.com/drive/u/0/folders/1VwFSTDr7e30YQw8UJlvbvhGeznAHdRyb
gdown https://drive.google.com/drive/u/0/folders/1VwFSTDr7e30YQw8UJlvbvhGeznAHdRyb
```
In case the `gdown` commands don't work, please directly download from the links.
### Extract 

```
gzip -d GoogleNews-vectors-negative300.bin.gz
unzip semeval2019-task8-dataset.zip
tar -xf semeval2019-task8-test-data.zip

```
