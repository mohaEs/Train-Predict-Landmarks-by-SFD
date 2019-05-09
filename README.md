# Landmark detection by shapenet

This repository shows a detail example of using shapenet for landmark detection.
The original method and source repository is:
https://github.com/justusschock/shapenet
and please consider that first.


## set ups

- make sure python version is 3.6 , <br/>
i.e. have a conda environment with python 3.6

- install pytorch suitable version

- install delira this version <br/>
pip install delira[torch]==0.3.1

- install shapenet:<br/>
https://github.com/justusschock/shapenet

- download your kaggle.json from kaggle site and put in in C:\Users\User\.kaggle.<br/>
Generally you should get this file first from our homepage 
www.kaggle.com -> Your Account -> Create New API token. <br/>
This will download a ready-to-go JSON file to place in you 
[user-home]/.kaggle folder. <br/>
If there is no .kaggle folder yet, please create it first.



## Usage

1- first prepare data from a zip file: <br/>
$ python ./scripts/prepare_datasets.py --helen --hzip ./helen.zip --ddir ./prepared_helen

2-correct the config file and locate it <br/>
set pca path, data path, epochs, saving model path and etc. 

3- train it by <br/>
passing the config file as argument <br/>
$ python ./scripts/train_single_shapenet.py --config ./helen.config --verbose

4- predict <br/>
pas the input path, output path, weight file and config file locations as arguments: <br/>
$ python ./scripts/predict_from_net.py --in_path ./prepared_helen/helen/testset --out_path ./output_predict/  --weight_file  ./Trained_Model/checkpoint_best.pth --config_file ./helen.config  --visualize 

Output of our experiment: (it was just 5 epochs which was trained on a few images) <br/>

![Alt text](./output_predict/visualization/31681454_1.png?raw=true "Title")