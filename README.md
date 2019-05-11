# Shapenet for Landmark detection/localization
# Train and predict

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
> python ./scripts/prepare_datasets.py --helen --hzip ./helen.zip --ddir ./prepared_helen

2-correct the config file and locate it <br/>
set pca path, data path, epochs, saving model path and etc. 

3- train it by <br/>
passing the config file as argument <br/>
> python ./scripts/train_single_shapenet.py --config ./helen.config --verbose

4- predict <br/>
pas the input path, output path, weight file and config file locations as arguments: <br/>
> python ./scripts/predict_from_net.py --in_path ./prepared_helen/helen/testset --out_path ./output_predict/  --weight_file  ./Trained_Model/checkpoint_best.pth --config_file ./helen.config  --visualize 

Output of our experiment: (it was just 5 epochs which was trained on a few images) <br/>

![Alt text](./output_predict/visualization/31681454_1.png?raw=true "Title")


## hint: Converting dataset 
This an exemplary code to create a zip file called helen.zip from your own dataset and landmarks which suitable format for training/predicting by above mentioned procedure.

It is supposed our image files (.png) and landmark files (.csv) are in [path_train_data '\temp_train_png\'] and [ path_train_data '\temp_train_lm\'] folders with the same name correspondigly. My n_points was 23.


```
validation_ratio=0.05;

path_train_png=[path_train_data '\temp_train_png\'];
path_train_lm=[ path_train_data '\temp_train_lm\'];

Path_write_train=[path_train_data '\helen\trainset\'];
Path_write_test=[path_train_data  '\helen\testset\'];
mkdir(Path_write_train);
mkdir(Path_write_test);

DirPngs=dir(fullfile(path_train_png,'*.png'));

Num_samples=length(DirPngs);
Ind_val=randperm(Num_samples,round(Num_samples*validation_ratio));

for SampleCounter=1:Num_samples

  ImageFileName= DirPngs(SampleCounter).name;
  FileName=ImageFileName(1:end-3); 
  LMs=csvread([path_train_lm ImageFileName(1:end-4) '.csv']);
  if isempty(find(SampleCounter==Ind_val))
    fileID = fopen([Path_write_train '\' FileName 'pts'],'w');
      copyfile([path_train_png ImageFileName],[Path_write_train ImageFileName]);
  else
    fileID = fopen([Path_write_test '\' FileName 'pts'],'w');
      copyfile([path_train_png ImageFileName],[Path_write_test ImageFileName]);
  end

  fprintf(fileID, "version: 1 \n");
  fprintf(fileID, "n_points:  23 \n");
  fprintf(fileID, "{ \n");
  for lm_counter=1:length(LMs)
     fprintf(fileID, "%d %d \n",LMs(lm_counter,1),LMs(lm_counter,2));
  end

  fprintf(fileID, "} \n");      
  fclose(fileID);


end

zippedfiles = zip([path_train_data 'Fold_' num2str(FoldCounter) '\helen.zip'] ,[path_train_data 'Fold_' num2str(FoldCounter) '\helen\']);
status = rmdir([path_train_data 'Fold_' num2str(FoldCounter) '\helen\'], 's');


```



