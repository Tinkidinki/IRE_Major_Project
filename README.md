# branchLSTM

This repo has been cloned from [here](https://github.com/kochkinaelena/branchLSTM).
- The 2017 winning code of the Semeval competition has been trained and evaluated on the 2019 contest Twitter data. 
- A few changes have been made, including changing the data into the right format, and resolving a few errors. 
- Please read this README file carefully to run the data. For the original README, look at the aforementioned repository.
- In this code, we use the optimal hyperparameters as found in the below research paper. 

This code is the implementation of the branch-LSTM model from the paper
*Turing at SemEval-2017 Task 8: Sequential Approach to Rumour Stance
Classification with Branch-LSTM*, available [here](
https://www.aclweb.org/anthology/S/S17/S17-2083.pdf).

This version of the code uses Python 2.7 with the Lasagne and Theano libraries.


## Table of Contents

- [Option 1: Installation on a local machine](#option-1-installation-on-a-local-machine)
- [Option 2: Installation on a Microsoft Azure VM with GPU](#option-2-installation-on-a-microsoft-azure-vm-with-gpu)
- [Run with the hyperparameters used in the paper](#run-with-the-hyperparameters-used-in-the-paper)
- [Run with a new set of optimised hyperparameters](#run-with-a-new-set-of-optimised-hyperparameters)
- [Code structure](#code-structure)
- [Contact](#contact)


To begin, clone this repository.
```
git clone https://github.com/Tinkidinki/branch-lstm-testing.git
```

The datasets from the SemEval-2019  challenge and a Word2Vec model pretrained on the Google News dataset are required.

These files should be placed in the `downloaded_data` folder.
Instructions for acquiring these files may be found in the [README](downloaded_data/README.md) inside the `downloaded_data` folder.

We recommend creating a new virtual environment and installing the required packages via `pip`.
```
cd <your-branchLSTM-directory>
virtualenv env
source env/bin/activate
pip install -r requirements.txt
```

## Run with the hyperparameters used in the paper

This option can be run on machines both with and without a GPU, but you may find subtly different results from the paper when using approaches other than that described in the Option 2 installation instructions.

* Run the preprocessing stage to convert the data into a format that is compatible with Lasagne.
```
python2 preprocessing.py
```
* Construct the model using the optimal set of hyperparameters and apply to the test dataset. The hyperparameters will be loaded from `output/bestparams_semeval2017.txt`.
```
THEANO_FLAGS='floatX=float32' python2 outer.py
```
The results are saved in `output/predictions.txt` in a format compatible with the scoring script.
* Evaluate the performance of the model with the official SemEval-2017 scoring script (this script uses Python 3 rather than Python 2, so we specify the correct Python version).
```
python3 scorer/scorerA.py "subtaska.json" "output/predictions.txt"
```
* We can also generate the results that populate most of the tables in the paper (a couple of these, indicated in the output, require files generated during the hyperparameter optimisation process).
```
python depth_analysis.py
```



## Code structure

`preprocessing.py`
  * Preprocesses the data into format suitable for use with Lasagne and saves arrays into the `saved_data` folder
  * `load_dataset` loads data into a python dictionary
  * `tree2branches` splits tree into branches
  * `cleantweet` replaces urls and pictures in the tweet with `picpicpic` and `urlurlurl` tokens
  * `str_to_wordlist` applies cleantweet and tokenizes the tweet, optionally removing stopwords
  * `loadW2vModel` loads W2V model into global variable
  * `sumw2v` turns tweet into sum or average of its words' vectors
  * `getW2vCosineSimilarity` computes cosine similarity between tweets
  * `tweet2features` extracts features from tweet
  * `convertlabel` converts `str` labels to `int`

`training.py`
  * `build_nn` defines the architecture of the Neural Network
  * `iterate_minibatches` splits the training data into mini-batches and returns iterator
  * `objective_train_model` trains the model on the training set, evaluates on development set and returns output in a format suitable for use with the `hyperopt` package

`predict.py`
  * `eval_train_model` re-trains the model on training and development set and evaluates on the test set

`outer.py`
  * `parameter_search` defines parameter space, performs parameter search using `objective_train_model` and `hyperopt` TPE search.
  * `convertlabeltostr` converts `int` labels to `str`
  * `eval` passes parameters to `eval_train_model`, does results postprocessing to fit with `scorer.py` and saves results
  * `main` brings all together, controls command line arguments
  * The following options are available:
    * `--search`: boolean, controls whether parameter search should be performed (default `--search=False`)
    * `--ntrials`: if `--search=True` then this controls how many different parameter combinations should be assessed (default `--ntrials=10`)
    * `--test`: boolean, if `--search=True`, this sets the type of parameter search to be run (default `--test=False`;)
    * `--params`: specifies filepath to file with parameters if `--search=False` (default `--params=output/bestparams_semeval2017`)
    * `-h`, `--help`: explains the command line arguments


`bestparams_semeval2017.txt`
  * This file stores the parameters used in the competition and paper


## Contact

Feel free to email E.Kochkina@warwick.ac.uk if you have any questions.
