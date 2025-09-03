# Counterfactual Analysis Driven Unsupervised Image Editing in Stable Diffusion

## Training

### Classifier
Step 1: To TRAIN a classifier and when you think the accuracy of classifier is enough, you can pause training process:
python train_classifier.py train \
   --data_dir "/home/ids/ziliu-24/TIME_copy/ffhq_glasses" \
   --model_path "/home/ids/ziliu-24/TIME_copy/ffhq_glasses/best_model.pth" \
   --output_csv_path "/home/ids/ziliu-24/TIME_copy/ffhq_glasses/true_label.csv" \
   --plots_path "/home/ids/ziliu-24/TIME_copy/ffhq_glasses/training_plots.png" \
   --num_classes 2 \
   --batch_size 64 \
   --lr 0.0001 \
   --epochs 50 \
   --patience 10

Step 2: To PREDICT with a trained classifier to get csv file:
python train_classifier.py predict \
   --data_dir "/home/ids/ziliu-24/TIME_copy/ffhq_glasses" \
   --model_path "/home/ids/ziliu-24/TIME_copy/ffhq_glasses/best_model.pth" \
   --csv_output_path "utils/ffhq_glasses-train-prediction-label-0.csv" \
   --num_classes 2 \
   --batch_size 64

### Pseudo-word embedding
Step 1: To train the **context** embedding, run the code as follows:

```bash
DATASET=name-your-dataset
PATHDATA=/path/to/data
CONTEXTTOKENS=context.pth  # output filename
LQ=0  # query label to train on, e.g. 0 for forward/stop binary task in bdd
CUSTOMTOKENS="'|<C*1>|' '|<C*2>|' '|<C*3>|'"
INITTOKENS="unknown unknown unknown"

python training.py \
    --output_path $CONTEXTTOKENS \
    --dataset $DATASET \
    --data_dir $PATHDATA \
    --label_query $LQ --training_label -1 \
    --custom_tokens $CUSTOMTOKENS \
    --custom_tokens_init $INITTOKENS \
    --phase context \
    --mini_batch_size 1 \
    --enable_xformers_memory_efficient_attention
```
Here, the SD model will learn the text embeddings linked with the |<C*1>|, |<C*2>|, and |<C*3>| text code. These embeddings will be warmed-up with the embeddings coupled with the words in INITTOKENS. The output is a small ``.pth`` file containing the token code and its learned text embedding.

## 📝 Notes
Our method is inspired by the idea proposed in [Text-to-Image Models for Counterfactual Explanations: a Black-Box Approach](https://arxiv.org/abs/2309.07944)

⭐️ Feel free to star this repo and reach out with any issues or suggestions!
