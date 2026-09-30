<img src="/README_images/screenshot-to-code.svg?raw=true" width="800px">

---

**A detailed tutorial covering the code in this repository:** [Turning design mockups into code with deep learning](https://emilwallner.medium.com/how-you-can-train-an-ai-to-convert-your-design-mockups-into-html-and-css-cc7afd82fed4).

**Plug:** 👉 Check out my 60-page guide, [No ML Degree](https://www.emilwallner.com/p/no-ml-degree), on how to land a machine learning job without a degree.

The neural network is built in three iterations. Starting with a Hello World version, followed by the main neural network layers, and ending by training it to generalize. 

The models are based on Tony Beltramelli's [pix2code](https://github.com/tonybeltramelli/pix2code), and inspired by Airbnb's [sketching interfaces](https://airbnb.design/sketching-interfaces/), and Harvard's [im2markup](https://github.com/harvardnlp/im2markup).

**Note:** only the Bootstrap version can generalize on new design mock-ups. It uses 16 domain-specific tokens which are translated into HTML/CSS. It has a 97% accuracy. The best model uses a GRU instead of an LSTM. This version can be trained on a few GPUs. The raw HTML version has potential to generalize, but is still unproven and requires a significant amount of GPUs to train. The current model is also trained on a homogeneous and small dataset, thus it's hard to tell how well it behaves on more complex layouts.

Dataset: https://github.com/tonybeltramelli/pix2code/tree/master/datasets

A quick overview of the process: 

### 1) Give a design image to the trained neural network

![Insert image](https://i.imgur.com/LDmoLLV.png)

### 2) The neural network converts the image into HTML markup 

<img src="/README_images/html_display.gif?raw=true" width="800px">

### 3) Rendered output

![Screenshot](https://i.imgur.com/tEAfyZ8.png)


## Installation

### FloydHub

[![Run on FloydHub](https://static.floydhub.com/button/button.svg)](https://floydhub.com/run?template=https://github.com/floydhub/pix2code-template)

Click this button to open a [Workspace](https://blog.floydhub.com/workspaces/) on [FloydHub](https://www.floydhub.com/?utm_medium=readme&utm_source=pix2code&utm_campaign=aug_2018) where you will find the same environment and dataset used for the *Bootstrap version*. You can also find the trained models for testing.

### Local
``` bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
```
```
git clone https://github.com/emilwallner/Screenshot-to-code.git
cd Screenshot-to-code/
.venv/bin/jupyter notebook
```
Run the notebooks in this order, each from its own folder: `Hello_world/hello_world.ipynb` (tiny end-to-end demo), then `Bootstrap/bootstrap.ipynb` (trains the model, saves `model.keras`), then `Bootstrap/test_model_accuracy.ipynb` (BLEU eval + token-to-HTML compile, needs `model.keras` from the training step).

> Note: train `Bootstrap/bootstrap.ipynb` (50 epochs) on a GPU — e.g. Google Colab — and copy the resulting `model.keras` back into `Bootstrap/` before running the eval notebook. A 1-epoch CPU smoke test is enough to verify the pipeline runs.
Go do the desired notebook, files that end with '.ipynb'. To run the model, go to the menu then click on Cell > Run all

The final version, the Bootstrap version, is prepared with a small set to test run the model (`Bootstrap/resources/eval_light/`, 10 samples — enough to verify the pipeline, not to reach good accuracy). The original full dataset and pre-trained weights were hosted on FloydHub, which has since shut down, so those links below are dead; train `Bootstrap/bootstrap.ipynb` yourself to produce `model.keras`.

## Folder structure

``` bash
  |  |-Bootstrap                           #The Bootstrap version
  |  |  |-compiler                         #A compiler to turn the tokens to HTML/CSS (by pix2code)
  |  |  |-resources											
  |  |  |  |-eval_light                    #10 test images and markup
  |  |-Hello_world                         #The Hello World version
  |  |-HTML                                #The HTML version
  |  |  |-Resources_for_index_file         #CSS,images and scripts to test index.html file
  |  |  |-html                             #HTML files to train it on
  |  |  |-images                           #Screenshots for training
  |-readme_images                          #Images for the readme page
```


## Hello World
<p align="center"><img src="/README_images/Hello_world_model.png?raw=true" width="400px"></p>


## HTML
<p align="center"><img src="/README_images/HTML_model.png?raw=true" width="400px"></p>


## Bootstrap
<p align="center"><img src="/README_images/Bootstrap_model.png?raw=true" width="400px"></p>

## Model weights
- [Bootstrap](https://www.floydhub.com/emilwallner/datasets/imagetocode) (The pre-trained model uses GRUs instead of LSTMs)
- [HTML](https://www.floydhub.com/emilwallner/datasets/html_models)

## Acknowledgments
- Thanks to IBM for donating computing power through their PowerAI platform
- The code is largely influenced by Tony Beltramelli's pix2code paper. [Code](https://github.com/tonybeltramelli/pix2code) [Paper](https://arxiv.org/abs/1705.07962)
- The structure and some of the functions are from Jason Brownlee's [excellent tutorial](https://machinelearningmastery.com/develop-a-caption-generation-model-in-keras/)
