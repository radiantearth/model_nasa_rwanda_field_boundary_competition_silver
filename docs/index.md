# nasa: HungryLearner solution

This documents the following submission

 - ID: `suGqqCGZ`
 - Filename: `ens_former+3extras+1fapn_t9.csv`
 - Comment: `bawo ni leekan si???`
 - Submitted: `26 February 19:50`

This submission was made in the `NASA Harvest Field Boundary Detection Challenge` and placed second on the private leaderboard.

## Creators
This solution was developed by:

 - Bojesomo Alabi - [Gmail](mailto:asbojesomo@gmail.com)

## Contact

Use email to message.

## Citation
Bojesomo A. \"Harvest Ensemble Segmentation Model for Fields\", Version 1.0, Radiant MLHub. [Date Accessed] Radiant MLHub <https://doi.org/10.34911/rdnt.49rmmy>

## License

[CC-BY-4.0](../LICENSE)

## Environment

Below are details of the environment.

### Python

```
python --version
Python 3.8.16
```

### Packages

The relevant packages are shown below. This is the contents of the `requirements.txt` file that is included in the solution.

```
torch
rasterio==1.3.4
radiant_mlhub==0.5.5
scipy==1.8.1
einops
sklearn
ttach==0.0.3
kornia
pytorch_lightning
torchmetrics
timm
torch_optimizer
segmentation_models_pytorch==0.3.2
matplotlib
pandas
```
### Competition data

Note that the competition data is not included in the archive. It *has to be copied in* in order to reproduce the solution.

Copy the data into the

```
nasa_rwanda_field_boundary_competition
```

directory. This directory is in the archive, but it is just an empty placeholder initially.

When done, it should look something like this.

```
|--- <solution root>
|       |--- nasa_rwanda_field_boundary_competition
|       |       |--- nasa_rwanda_field_boundary_competition_labels_train
|       |       |       |--- _common
|       |       |       |--- collection.json
|       |       |       |--- nasa_rwanda_field_boundary_competition_labels_train_00
|       |       |       |--- nasa_rwanda_field_boundary_competition_labels_train_01
|       |       |       |--- nasa_rwanda_field_boundary_competition_labels_train_02
|       |       |       |--- nasa_rwanda_field_boundary_competition_labels_train_03
|       |       |            ...
|       |       |--- nasa_rwanda_field_boundary_competition_labels_test
|       |       |       |--- collection.json
|       |       |       |--- nasa_rwanda_field_boundary_competition_source_test_00_2021_03
|       |       |       |--- nasa_rwanda_field_boundary_competition_source_test_00_2021_04
|       |       |       |--- nasa_rwanda_field_boundary_competition_source_test_00_2021_08
|       |       |            ...
|       |--- README.md
|       |--- ensemble-top-submit.ipynb
|            ...
```

## Data Preparation
For simplicity, the data in ```nasa_rwanda_field_boundary_competition``` was converted to ```pkl``` format using 
```
Saving Images as pkl.ipynb
```
When done, it should look something like this.
```
|--- <solution root>
|       |--- data
|       |       |--- train
|       |       |       |--- fileds
|       |       |       |       |---00.pkl
|       |       |       |       |---01.pkl
|       |       |       |           ...
|       |       |       |--- masks
|       |       |       |       |---00.pkl
|       |       |       |       |---01.pkl
|       |       |       |           ...
|       |       |--- test
|       |       |       |--- fields
|       |       |       |       |---00.pkl
|       |       |       |       |---01.pkl
|       |       |--- statistics.csv
|       |       |--- test.csv
|       |       |--- train.csv
|       |--- README.md
|       |--- ensemble.ipynb
|            ...
```

## Solution review

The solution is based on the following pre-trained models
 - encoders
   - selecsls42b
   - selecsls60
   - sebotnet33ts_256
   - vgg13_bn
   - vgg19
   - vgg16
   - vgg19_bn
 - decoders
   - unet++
   - manet
   - [MANet](https://github.com/bojesomo/model_nasa_rwanda_field_boundary_competition_silver/tree/main/full_solution/decoders/MANet.py)
   - [fapn](https://github.com/bojesomo/model_nasa_rwanda_field_boundary_competition_silver/tree/main/full_solution/decoders/fapn.py)

The following variations were applied to this model. It was majorly trained with varied set of input channels and output types
 - input variation
   - visible only
     - band 1 (Blue)
     - band 2 (Green)
     - band 3 (Red)
   - all bands available
     - band 1 (Blue)
     - band 2 (Green)
     - band 3 (Red)
     - band 4 (NIR)
     
 - output variation
  - boundary only
  - boundary + extent
    - `extent` refers to filling all fields with 1 to mimic a pure segmentation mask 

### System

These models were optimised using.

 - Linux
 - 16GB NVIDIA GPU

It will take a few hours to run.

## Preprocessing

All channels were scaled using statistics computed from the training data.

## Validation

All models were trained with full data without validation using `sam` optimizer with `adamw` as the `base optimizer` to limit overfitting.

## Postprocessing
TTA
 - merge
   - max
 - ops
   - `HorizontalFlip`
   - `VerticalFlip`
   - `Rotate90`

## Running the code

### Model

Once the data is ready, you can run the solution by running the notebook.

```
Experiments.ipynb
```

### Submission

I apply threshold 0.9 then voting ensemble on each cell. The final submission file can be generated by running the notebook

```
ensemble.ipynb
```
