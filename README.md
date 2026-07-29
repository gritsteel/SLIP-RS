### Install Environment x 2
```bash
conda create -n remoteclip_ft python=3.10 -y
conda activate remoteclip_ft
pip install torch==2.4.1 torchvision==0.19.1 torchaudio==2.4.1 --index-url https://download.pytorch.org/whl/cu121
cd /SLIP-RS/RemoteCLIP_ft/
pip install -r requirements.txt
```

```bash
conda create -n sliprs python=3.10 -y
conda activate sliprs

pip install torch==1.13.0+cu116 torchvision==0.14.0+cu116 torchaudio==0.13.0 --extra-index-url https://download.pytorch.org/whl/cu116
pip install -U openmim
mim install mmcv-full==1.7.1
cd /SLIP-RS/mmdetection_sliprs/
pip install -r requirements.txt
pip install -v -e .
pip install ftfy regex numpy==1.26.1 yapf==0.40.1
```

### Data_1
Please download the dataset via [Baidu Cloud](https://pan.baidu.com/s/1XNscwBdndGjwih_zk8EzfQ)(udnm) and organize the dataset as follows:

```bash
RemoteCLIP_ft/
├── DATA/
│   ├── RS_Attri_Cls/
│   │   ├── train/
│   │   │   ├── image/
│   │   │   └── label/
│   │   └── test/
│   │       ├── image/
│   │       └── label/
```

### Data_2

SLIP-RS is trained on both open-source remote sensing datasets and large-scale curated datasets:

1. RS-O: 

- **DOTA-v2.0**. Please download the images and horizontal bounding box annotations from the official [DOTA dataset website](https://captain-whu.github.io/DOTA/dataset.html?utm_source=chatgpt.com). After downloading, preprocess the dataset by slicing large images into patches following the official tools provided by [MMRotate DOTA tools](https://github.com/open-mmlab/mmrotate/tree/main/tools/data/dota?utm_source=chatgpt.com). Then, convert the original TXT annotations into COCO-format JSON annotations. RS-O includes all train set.
- **DIOR**. Please download from [DIOR](https://gcheng-nwpu.github.io/#Datasets). Then, convert the original XML annotations into COCO-format JSON annotations. RS-O includes all trainval set.
- **Others**. Other open-source datasets included in RS-O can be downloaded from:[Baidu Cloud](https://pan.baidu.com/s/1-XQ69xTzGCdFlot_QzCnJg)(code: 7gcj)

2. RS-O-Attri

- The attribute annotations for RS-O can be downloaded from:[Baidu Cloud](https://pan.baidu.com/s/1Rlvk5j3XUR7XHDsjN8whCg)(code: 68yx)

3. RS-C & RS-C-Attri

- The large-scale curated dataset and its corresponding attribute annotations can be downloaded from:[Baidu Cloud](https://pan.baidu.com/s/1gI0BLTuWXMYmuMpm5G98tA)(code: kuw9). 
Some large files (e.g., `Asia`) are split into multiple parts. Please merge them using:

    ```bash
    cd Asia
    cat Asia.zip.part* > Asia.zip
    ...
    ```

4. Test Data

- **DOTA-v2.0**. Please download the images and horizontal bounding box annotations from the official [DOTA dataset website](https://captain-whu.github.io/DOTA/dataset.html?utm_source=chatgpt.com). After downloading, preprocess the dataset by slicing large images into patches following the official tools provided by [MMRotate DOTA tools](https://github.com/open-mmlab/mmrotate/tree/main/tools/data/dota?utm_source=chatgpt.com). We use all val set to test the performance of DOTA-v2.0.
- **DIOR**. Please download from [DIOR](https://gcheng-nwpu.github.io/#Datasets). We use all test set to test the performance of DIOR.
- **Attri_test**. The attribute annotations for Attri_test can be downloaded from:[Baidu Cloud](https://pan.baidu.com/s/1m6Nvq_i9MBShpGWbA3lYrQ)(code: snmm)

Finally, organize the dataset as follows:
```bash
path/to/your/data/
├── dota2/
│   ├── images/
│   ├── dota2_train_label.json
│   └── dota2_val_label.json
│ 
├── dior/
│   ├── trainval_images/
│   ├── dota2_train_label.json
│   ├── test_images/
│   └── dota2_test_label.json
│ 
├── RS_O/
│   ├── aitod2/
│   │   ├── images/
│   │   └── annotations.json
│   ├── dronevehicle/
│   │   ├── images/
│   │   └── annotations.json
│   │
│   │  ......
│   │
│   └── simd/
│       ├── images/
│       └── annotations_one.json
│
├── RS_Attri_O/
│   ├── images/
│   └── annotations.json
│
├── RS_C/
│   ├── Asia/
│   │   ├── images/
│   │   └── annotations.json
│   │   └── annotations_attribute.json
│   ├── Europe/
│   │   ├── images/
│   │   └── annotations.json
│   │   └── annotations_attribute.json
│   ├── North_America/
│   │   ├── images/
│   │   └── annotations.json
│   │   └── annotations_attribute.json
│   ├── Others/
│   │   ├── images/
│   │   └── annotations.json
│   ├── Others1/
│   │   ├── images/
│   │   └── annotations.json
│   └── Others_Attri/
│       ├── images/
│       └── annotations.json
│ 
└── Attribute_test/
    ├── plane/
    │   ├── images/
    │   └── annotations.json
    ├── ship/
    │   ├── images/
    │   └── annotations.json
    └── vehicle/
        ├── images/
        └── annotations.json
```

### Pretrain Weights
Please download the following pretrained checkpoints:
- **DINOv3-ConvNeXT-Tiny** from [DINOv3 repository](https://github.com/facebookresearch/dinov3)  
  → download: `DINOv3-ConvNeXT-Tiny`
- **DINOv3-ConvNeXT-Large** from [DINOv3 repository](https://github.com/facebookresearch/dinov3)  
  → download: `DINOv3-ConvNeXT-Large`
- **RemoteCLIP-FG** Our fine-tuned **RemoteCLIP-FG** checkpoint can be downloaded from:
[Google Drive](https://drive.google.com/file/d/1cEgcDZsyNZWRYzasrCKexooWd85EVcJu/view?usp=sharing&utm_source=chatgpt.com)

After downloading, put them in `model_weights` folder.



Put these three files in `mmdetection_sliprs\model_weights\`:

```text
dinov3_convnext_tiny_pretrain_lvd1689m-21b726bb.pth
remoteclip_ft.pth
SLIP_RS_T.pth
```

Use the new `slip-rs_convnext-t_lora-clip_fpn_1x_6gb.py` configuration. It keeps the SLIP-RS-T model but lowers the inference image size to 800 x 800, limits RPN proposals to 500, and returns at most 300 detections. It is an inference-speed/memory setting, not a paper-result reproduction setting.

```powershell
python .\tools\sliprs_infer_visualize.py .\sliprs_configs\slip-rs_convnext-t_lora-clip_fpn_1x_6gb.py .\model_weights\SLIP_RS_T.pth .\tools\plane.png --prompt "Plane+Twin-engine" "Plane+Four-engine" --score-thr 0.3 --out-dir .\sliprs_vis_results --device cuda:0
```

If CUDA runs out of memory, close GPU-heavy desktop applications first. If that is not enough, edit only `low_vram_img_scale` from `(800, 800)` to `(640, 640)` and `low_vram_rpn_proposals` from `500` to `300` in the low-VRAM configuration. This can reduce small-object recall. Do not use the SLIP-RS-L checkpoint on this GPU.

### Train
```bash
bash ./tools/dist_train.sh ./sliprs_configs/slip-rs_convnext-t_lora-clip_fpn_1x_rs-attri.py 8
```

### Test
Our pretrained model weights:
| Model     | Weight                                                                                                  |
|-----------|---------------------------------------------------------------------------------------------------------|
| SLIP-RS-T | [Pretrained Weight](https://drive.google.com/file/d/1_upTH-zclhUcB_CrS3iYuUAiuN5Y153x/view?usp=sharing) |
| SLIP-RS-L | [Pretrained Weight](https://drive.google.com/file/d/1enHOD4X827pgObkUG45hU9H6bPtiixeL/view?usp=sharing) |


```bash
bash ./tools/dist_test.sh ./sliprs_configs/slip-rs_convnext-t_lora-clip_fpn_1x_rs-attri.py ./path/to/SLIP_RS_T.pth 8 --eval bbox
```

### Visualization (paper/default configuration)
```bash
python ./tools/sliprs_infer_visualize.py ./sliprs_configs/slip-rs_convnext-t_lora-clip_fpn_1x.py ./path/to/SLIP_RS_T.pth ./tools/plane.png --prompt ['plane+twin-engines', 'plane+four-engines'] --out-dir ./
```

You can test using any number and combination of the attributes found in the following dictionary, arranged in any order:
```bash
attri_dict = {"Plane" : {'Engine position': ['At wing roots and lower fuselage', 'Beneath the wings',
                                            'On the nose', 'Rear fuselage', 'Above the wings', 'Embedded within wing'],
                        'Number of engines': ['Eight-engine', 'Four-engine', 'One-engine', 'Twin-engine', 'Ten-engine'],
                        'Propulsion type': ['Jet', 'Propeller'],
                        'Purpose': ['AerialSupport Aircraft', 'Airborne Early Warning Aircraft', 'Airline Aircraft',
                                    'Anti-Submarine Warfare Aircraft', 'Bomber', 'Chartered aircraft', 'Fighter',
                                    'Propeller', 'Trainer', 'Transport Aircraft', 'Attack aircraft'],
                        'Usage': ['Civilian Aircraft', 'Commercial Aircraft', 'Military Aircraft'],
                        'Wing configuration': ['Straight wing', 'Swept delta wing', 'Swept diamond-like wing',
                                                'Swept wing', 'Swept, variable-sweep wing', 'Flying wing']},
            "Ship" : {'Usage': ['Civilian Ship', 'Commercial Ship', 'Engineering Ship', 'Military Ship'],
                        'Subcat': ['Barge', 'Container Ship', 'Dry Cargo Ship',
                                    'Cruise Ship', 'Liquid Cargo Ship', 'RoRo', 'Yacht'],
                        'Purpose': ['Aircraft Carrier', 'Amphibious Ship', 'Auxiliary Ship', 'Cargo Ship', 'Commander', 
                                    'Cruiser', 'Destroyer', 'Frigate', 'Landing', 'Medical Ship', 'Military Transport Ship', 
                                    'Passenger Ship', 'Patrol', 'Submarine', 'Test ship', 'Training ship', 'Tugboat',
                                    'Fishing Vessel', 'Motorboat']},
            "Vehicle" : {'Purpose': ['Bus', 'Cargo Truck', 'Dump Truck', 'Excavator', 'Pick-up', 'Small Passenger Car',
                                     'Tractor', 'Truck Tractor', 'Van'],
                         'Usage': ['Engineering Vehicle', 'Large Civilian Vehicle', 'Small Civilian Vehicle', 'Truck']}}
```

### Image
<img width="1024" height="1024" alt="plane" src="https://github.com/user-attachments/assets/29be66b5-82b4-4534-96f5-815cf59e6951" />


### No data demo 
```
./tools/run_plane_demo.sh
```

```bash
/home/me/miniforge3/envs/sliprs/lib/python3.10/site-packages/mmcv/__init__.py:20: UserWarning: On January 1, 2023, MMCV will release v2.0.0, in which it will remove components related to the training process and add a data transformation module. In addition, it will rename the package names mmcv to mmcv-lite and mmcv-full to mmcv. See https://github.com/open-mmlab/mmcv/blob/master/docs/en/compatibility.md for more details.                                                                                                warnings.warn(                                                                                        Residual Attention Block 0: ResidualAttentionBlock(                                                       (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 1: ResidualAttentionBlock(                                                       (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 2: ResidualAttentionBlock(                                                       (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 3: ResidualAttentionBlock(                                                       (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 4: ResidualAttentionBlock(                                                       (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 5: ResidualAttentionBlock(                                                       (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 6: ResidualAttentionBlock(                                                       (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 7: ResidualAttentionBlock(                                                       (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 8: ResidualAttentionBlock(                                                       (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 9: ResidualAttentionBlock(                                                       (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 10: ResidualAttentionBlock(                                                      (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Residual Attention Block 11: ResidualAttentionBlock(                                                      (attn): MultiheadAttention(                                                                               (out_proj): NonDynamicallyQuantizableLinear(in_features=512, out_features=512, bias=True)             )                                                                                                       (ln_1): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                           (mlp): Sequential(                                                                                        (c_fc): Linear(in_features=512, out_features=2048, bias=True)                                           (gelu): QuickGELU()                                                                                     (c_proj): Linear(in_features=2048, out_features=512, bias=True)                                       )                                                                                                       (ln_2): LayerNorm((512,), eps=1e-05, elementwise_affine=True)                                         )                                                                                                       Text encoder params: trainable=0 / total=63,501,825                                                     load checkpoint from local path: /workspace/SLIP-RS-main/SLIP-RS-main/mmdetection_sliprs/model_weights/SLIP_RS_T.pth                                                                                            Saved visualization: /workspace/SLIP-RS-main/SLIP-RS-main/mmdetection_sliprs/tools/sliprs_vis_results/plane_vis.png 
```

### Result
<img width="1024" height="1024" alt="plane_vis" src="https://github.com/user-attachments/assets/6b10e8f8-2cb4-4c54-89ef-0b1471289ecb" />
