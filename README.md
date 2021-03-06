# EssaySense

**EssaySense** (former name: AES) is an NLP project on Automated Essay Scoring, based on neural network technologies.

**Authors**: _[Zilong Liang](https://github.com/zlliang)_ and _[Jiancong Gao](https://github.com/jianconggao)_.

Several neural network models are included to modelling the scoring task, implemented using [TensorFlow](https://tensorflow.org). Pre-trained models are also included based on [ASAP-AES](https://www.kaggle.com/c/asap-aes/) dataset. You can use this application to score English essays, or train new models by feeding your own datasets.

## Requirements

[Python 3.5+](https://python.org), [TensorFlow 1.4.1+](https://tensorflow.org) and [NLTK 3.2+](http://www.nltk.org) are required to run neural network models, and [click 6.7+](http://click.pocoo.org/6/) is required to run the command line application. You can install all of the dependencies through the following command, if you are using [pip](https://pip.pypa.io/) as your python package manager:
```bash
$ pip3 install -r requirements.txt
```

Besides, NLTK's punctuation metadata is needed to perform sentence tokenizing task. But **do not** bother to download it manually, because this application would detect or download it automatically.

For **datasets** part, this project uses `training_set_rel3.tsv` in [ASAP-AES](https://www.kaggle.com/c/asap-aes/) as essay dataset and `glove.6B.50d.txt` of [GloVe](https://github.com/stanfordnlp/GloVe) project as word embedding dataset. They are about approximate 200MB totally, so you need prepare them yourself, and put them into: `essaysense/datasets/`.

## Usage

Check out the root directory of this project. A Command Line Interface named `essaysense-cli` is delivered to run the project. It's developed based on [click](http://click.pocoo.org/6/). This CLI application could perform several tasks, including listing avaliable models, training, evaluating and visualizing. The follwing block gives an overview on usage.
```bash
$ ./essaysense-cli --help  # Show help information.
$ ./essaysense-cli --version  # Show the current version of the app.

$ ./essaysense-cli show  # Show names of avaliable AES models.
$ ./essaysense-cli train (model-name) [--prompt (domain-id)]  # Train a model from the beginning.
$ ./essaysense-cli evaluate (model-name) [--prompt (domain-id)]  # Run test on a specific pre-trained model.
$ ./essaysense-cli visualize (model-name) [--prompt (domain-id)]  # Visualize training process in TensorBoard.
```

In detail, suppose that your want to see avaliable AES models, you can use the command `show`. Note that these model names are important if you want to train them:
```bash
$ ./essaysense-cli show
[Loading] Avaliable models...
1: cnn-cnn
2: cnn-lstm
3: lstm
```

Then, training models seems fairly easy:
```bash
$ ./essaysense-cli train lstm -p 7
[Loading] GloVe word vectors...
[Loading] ASAP-AES domain 7 dataset...
[Loading] ASAP-AES domain 7 dataset...
[Training] prompt-7-document-level-lstm-with-mot-pooling
Train:   1,   Loss: 0.058921,   QWK-on-dev-set: 0.228258
Train:   2,   Loss: 0.019410,   QWK-on-dev-set: 0.332542
Train:   3,   Loss: 0.018675,   QWK-on-dev-set: 0.173433
...
```
Note that `-p` option is short for `--prompt`, identifying which prompt of the ASAP-AES dataset you would like to feed the model as a train set, the default value is `1`.

Evaluation can be performed after the model on a specific prompt is appropriately trained:
```bash
$ ./essaysense-cli evaluate cnn-cnn -p 2
[Loading] GloVe word vectors...
[Loading] ASAP-AES domain 2 dataset...
[Loading] ASAP-AES domain 2 dataset...
[Evaluating prompt-2-sentence-level-cnn] QWK-on-test-set: 0.4919167
```

This application also provides you a simple interface to access TensorBoard along with a specific trained model:
```bash
$ ./essaysense-cli visualize cnn-cnn -p 2
[Loading] GloVe word vectors...
[Loading] ASAP-AES domain 2 dataset...
[Loading] ASAP-AES domain 2 dataset...
[Visualizing] Calling Tensorboard...
TensorBoard 0.4.0 at http://localhost:6006 (Press CTRL+C to quit)
```
Obviously, you have to add TensorBoard to you `$PATH` enviroment so that the application could make it.

Note that both evaluating and visualizing tasks demand a trained model on a single prompt. If you call a brand-new model for evaluating or visualizing, It would reporting error:
```bash
$ ./essaysense-cli visualize lstm -p 5
[Loading] GloVe word vectors...
[Loading] ASAP-AES domain 5 dataset...
[Loading] ASAP-AES domain 5 dataset...
[Visualizing] Calling Tensorboard...
[Error] The model have not been trained. Please train first.
```

### Notes

1. All of the dataset used for this project should be download manually following instructions above.
2. [GloVe](https://github.com/stanfordnlp/GloVe) word representation resources are licensed under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0) and pre-trained embeddings are licensed under the [Public Domain Dedication and License](https://opendatacommons.org/licenses/pddl/).
3. This project is developed under the [MIT](https://mit-license.org) license.
