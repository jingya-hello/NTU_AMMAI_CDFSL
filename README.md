# NTU-AMMAI-CDFSL

The source code of NTU-AMMAI-CDFSL.

### Datasets
   * mini-ImageNet: 
   * EuroSAT: https://drive.google.com/file/d/1DUj510w7726Iga34gmmhLA4meVYULrBe/view?usp=sharing
   * ISIC: https://drive.google.com/file/d/15jXP_rDEi_eusIK-kCz-DD5ZBFD3WeGF/view?usp=sharing

### Pretrained Model
   * ResNet10 Baseline/ProtoNet are provided in logs/checkpoints/miniImageNet.

## Description
   See https://docs.google.com/document/d/1rNuAb3D0dcXI776eKrj8iNpE2LQmCE63WU7lRTOQGIU/edit?usp=sharing

### Specific Tasks:

   **EuroSAT**

     • Shots: n = {5}

   **ISIC2018**

     • Shots: n = {5}


### Environment
   Python 3.7
   Pytorch 1.3.1

### Steps
   1. Download all the needed datasets via above links.
   2. Change configuration in config.py to the correct paths in your own computer.
   3. Train models on miniImageNet. (Note: You can only train your own model, other pretrained models are provided.)
   - **Standard supervised learning on miniImageNet**

       ```bash
           python ./train.py --dataset miniImageNet --model ResNet10  --method baseline --train_aug
       ```
   - **Train meta-learning method (protonet) on miniImageNet**
   The available method list: [protonet]

       ```bash
           python ./train.py --dataset miniImageNet --model ResNet10  --method protonet --n_shot 5 --train_aug
       ```
   4. Test
      You should know the following options:

      • --track: fsl/cdfsl, option for track 1(fsl) or track 2(cdfsl).

      • --model: ResNet10, network architecture.

      • --method: baseline/protonet/your-own-model.

      • --train_aug: add this if you train the model with this option.

      • --freeze_backbone: add this for inferring directly. (Do not add this if you want to fine-tune your model, you can only fine-tune models in track 2.)

      There are two meta-test files:

      * **meta_test_Baseline.py**:
      - For Baseline, we will train a new linear classifier using support set.

       ```bash
           python meta_test_Baseline.py --track cdfsl --model ResNet10 --method baseline  --train_aug --freeze_backbone
       ```
      - You can also train a new linear layer and fine-tune the backbone.

       ```bash
           python meta_test_Baseline.py --track cdfsl --model ResNet10 --method baseline  --train_aug
       ```

     * **meta_test_few_shot_models.py**:
     This method will apply the pseudo query set to the few-shot model you want to fine-tune with. (if add --freeze_backbone)

     The available method list: [protonet]

     The available model list: [ResNet10/ResNet18]

    ```bash
        python meta_test_few_shot_models.py --track cdfsl --model ResNet10 --method protonet  --train_aug --freeze_backbone
    ```

    No matter which finetune method you chosse, a dataset contains 600 tasks.

    After evaluating 600 times, you will see the result like this: 600 Test Acc = 49.91% +- 0.44%.

### Results

| Models  | miniImageNet | EuroSAT | ISIC |
| ------------- | ------------- | ------------- | ------------- |
| Baseline | 68.10% ± 0.67%* | 75.69% ± 0.66% / 79.08% ± 0.61% | 43.56% ± 0.60% / 48.11% ± 0.64% | 
| ProtoNet | 66.33% ± 0.65%* | 77.45% ± 0.56% / 81.45% ± 0.63% | 41.73% ± 0.56% / 46.72% ± 0.59% |

For miniImageNet, the accuracies are inconsistency with previous work's results. We are still looking for the reason.
For EuroSAT and ISIC, the result w/ and w/o fine-tuning are the first and second accuracy, respectively.

### TODOs
   1. Try to re-run all baseline models for both tracks. 

      ```bash
           python meta_test_Baseline.py --track fsl --model ResNet10 --method baseline  --train_aug --freeze_backbone
      ```

      ```bash
           python meta_test_Baseline.py --track cdfsl --model ResNet10 --method baseline  --train_aug 
      ```

      ```bash
           python meta_test_few_shot_models.py --track fsl --model ResNet10 --method protonet  --train_aug --freeze_backbone
      ```

      ```bash
           python meta_test_few_shot_models.py --track cdfsl --model ResNet10 --method protonet  --train_aug
      ```

   2. Design your own model, and report your results for both tracks.
      - You should inherit the template in meta_template.py, and design your own model.
      - For track 2, you can infer the query set directly, or you can also design your fine-tuning method (You can stil use pseudo query set to fine-tune).

       ```bash
           python meta_test_few_shot_models.py --track fsl --model ResNet10 --method your_method  --train_aug --freeze_backbone
      ```

      ```bash
           python meta_test_few_shot_models.py --track cdfsl --model ResNet10 --method your_method  --train_aug
      ```

### Contact Information
   H.T. Su (d06944009@ntu.edu.tw)

   Jia-Fong Yeh (jiafongyeh@ieee.org)