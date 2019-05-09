
-make sure python version is 3.6 , 
i.e. have a conda environment with python 3.6

-install pytorch suitable version

-install delira this version
pip install delira[torch]==0.3.1

- install shapenet by pip:
https://github.com/justusschock/shapenet

- download your kaggle.json from kaggle site and put in in C:\Users\User\.kaggle.
Generally you should get this file first from our homepage 
www.kaggle.com -> Your Account -> Create New API token. 
This will download a ready-to-go JSON file to place in you 
[user-home]/.kaggle folder. If there is no .kaggle folder yet, please create it first.


3- use terminal:
3-1- first prepare data from zip file:
$ python ./scripts/prepare_datasets.py --helen --hzip ./helen.zip --ddir ./prepared_helen

3-2-correct the config file and locate it
set pca path, data path in it and img size.
train it

$ python ./scripts/train_single_shapenet.py --config ./helen.config --verbose


3-3- predict

$ python ./scripts/predict_from_net.py --in_path ./prepared_helen/helen/testset --out_path ./output_predict/  --weight_file  ./Trained_Model/checkpoint_best.pth --config_file ./helen.config  --visualize 

