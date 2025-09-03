# Industrial Anomaly Detection
---

## Install

```shell
$ pipenv install -r requirements.txt
```

## Usage

CLI:
```shell
$ python run.py METHOD [--dataset DATASET]
```
Results can be found under `./results/`.

Code example:
```python
from indad.model import SPADE

model = SPADE(k=5, backbone_name="resnet18")

# get some training data
class_name = "bottle"
train_ds, test_ds = MVTecDataset(class_name).get_dataloaders()

model.fit(train_ds)

# evaluate
image_rocauc, pixel_rocauc = model.evaluate(test_ds)
print(image_rocauc, pixel_rocauc)
```

### Custom datasets
<details>
  <summary> 👁️ </summary>

Check out one of the downloaded MVTec datasets.
Naming of images should correspond among folders.
Right now there is no support for no ground truth pixel masks.

```
📂datasets
 ┗ 📂your_custom_dataset
  ┣ 📂 ground_truth/defective
  ┃ ┣ 📂 defect_type_1
  ┃ ┗ 📂 defect_type_2
  ┣ 📂 test
  ┃ ┣ 📂 defect_type_1
  ┃ ┣ 📂 defect_type_2
  ┃ ┗ 📂 good
  ┗ 📂 train/good
```

```shell
$ python run.py METHOD --dataset your_custom_dataset
```
</details>

---

## Results

📝 = paper, 👇 = this repo

### Image-level

| class      | SPADE 📝 | SPADE 👇 | PaDiM 📝   | PaDiM 👇 | PatchCore 📝 | PatchCore 👇 |
|-----------:|:--------:|:--------:|:----------:|:--------:|:------------:|:------------:|
| bottle     | -        | 98.8     |            | 99.8     | ■**100.0**■  | ■**100.0**■  |
| cable      | -        | 76.5     |            | 93.3     | ■**99.5**■   | 96.2         |
| capsule    | -        | 84.6     |            | 88.3     | 98.1         | 95.3         |
| carpet     | -        | 84.3     |            | ■**99.4**| 98.7         | 98.7         |
| grid       | -        | 37.1     |            | 98.2     | ■**98.2**■   | 93.0         |
| hazelnut   | -        | 88.7     |            | 83.7     | ■**100.0**■  | 100.0        |
| leather    | -        | 97.1     |            | 99.9     | ■**100.0**■  | 100.0        |
| metal_nut  | -        | 74.6     |            | 99.4     | ■**100.0**■  | 98.3         |
| pill       | -        | 72.6     |            | 89.0     | ■**96.6**■   | 92.8         |
| screw      | -        | 53.1     |            | 83.0     | 98.1         | 96.7         |
| tile       | -        | 97.8     |            | 98.6     | 98.7         | ■**99.0**■   |
| toothbrush | -        | 89.4     |            | 97.2     | ■**100.0**■  | 98.1         |
| transistor | -        | 89.2     |            | 96.8     | ■**100.0**■  | 99.7         |
| wood       | -        | 98.3     |            | 98.9     | ■**99.2**■   | 98.8         |
| zipper     | -        | 96.7     |            | 89.5     | ■**99.4**■   | 98.4         |
| averages   | 85.5     | 82.6     |  95.3*     | 94.3     | ■**99.1**■   | 97.7         |

* PaDiM average referencing PaDiM-WR50-Rd550

### Pixel-level

| class      | SPADE 📝   | SPADE 👇   | PaDiM 📝 | PaDiM 👇| PatchCore 📝   | PatchCore 👇 |
|-----------:|:----------:|:----------:|:--------:|:-------:|:--------------:|:------------:|
| bottle     | 97.5       | 97.7       | 98.3     | 97.8    | ■**98.6**■     | 97.8         | 
| cable      | 93.7       | 94.3       | 96.7     | 96.1    | ■**98.5**■     | 97.4         | 
| capsule    | 97.6       | 98.6       | 98.5     | 98.3    | ■**98.9**■     | 98.3         | 
| carpet     | 87.4       | 99.0       | 99.1     | 98.6    | ■**99.1**■     | 98.3         | 
| grid       | 88.5       | 96.1       | 97.3     | 97.2    | ■**98.7**■     | 96.7         | 
| hazelnut   | 98.4       | 98.1       | 98.2     | 97.5    | ■**98.7**■     | 98.1         | 
| leather    | 97.2       | 99.2       | 99.2     | 98.7    | ■**99.3**■     | 98.4         | 
| metal_nut  | ■**99.0**■ | 96.1       | 97.2     | 96.5    | 98.4           | 96.2         | 
| pill       | ■**99.1**■ | 93.5       | 95.7     | 93.2    | 97.6           | 98.7         | 
| screw      | 98.1       | 98.9       | 98.5     | 97.8    | ■**99.4**■     | 98.4         | 
| tile       | ■**96.5**■ | 93.3       | 94.1     | 94.8    | 95.9           | 94.0         | 
| toothbrush | ■**98.9**■ | ■**98.9**■ | 98.8     | 98.3    | 98.7           | 98.1         | 
| transistor | ■**97.9**■ | 96.3       | 97.5     | 97.2    | 96.4           | 97.5         | 
| wood       | 94.1       | 94.4       | 94.7     | 93.6    | ■**95.1**■     | 91.9         | 
| zipper     | 96.5       | 98.2       | 98.5     | 97.4    | ■**98.9**■     | 97.6         | 
| averages   | 96.9       | 96.8       | 97.5     | 96.9    | ■**98.1**■     | 97.2         |

__PatchCore-10 was used.__

### Hyperparams

The following parameters were used to calculate the results. 
They more or less correspond to the parameters used in the papers.

```yaml
spade:
  backbone: wide_resnet50_2
  k: 50
padim:
  backbone: wide_resnet50_2
  d_reduced: 250
  epsilon: 0.04
patchcore:
  backbone: wide_resnet50_2
  f_coreset: 0.1
  n_reweight: 3
```

---

## Progress

- [x] Datasets
- [x] Code skeleton
- [ ] Config files
- [x] CLI
- [x] Logging
- [x] SPADE
- [x] PADIM
- [x] PatchCore
- [x] Add custom dataset option
- [x] Add dataset progress bar
- [x] Add schematics
- [ ] Unit tests

## Design considerations

- Data is processed in single images to avoid batch statistics interference.
- I decided to implement greedy kcenter from scratch and there is room for improvement.
- `torch.nn.AdaptiveAvgPool2d` for feature map resizing, `torch.nn.functional.interpolate` for score map resizing.
- GPU is used for backbones and coreset selection. GPU coreset selection currently runs at:
  - 400-500 it/s @ float32 (RTX3080)
  - 1000+ it/s @ float16 (RTX3080)

---

## Streamlit demo

Run `$ streamlit run streamlit_app.py`

---

## Acknowledgements

-  [hcw-00](https://github.com/hcw-00) for tipping `sklearn.random_projection.SparseRandomProjection`.
-  [h1day](https://github.com/h1day) for adding a custom range to the streamlit app.
- MVTec dataset from https://www.mvtec.com/company/research/datasets/mvtec-ad, please note that this data is CC BY-NC-SA 4.0.