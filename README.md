Approach Note:
The objective was to develop a color-invariant saree design recognition system for image retrieval and verification. Since the normal dataset folder was empty, I used the available Kaggle saree datasets, which required extensive cleaning and restructuring. An initial dataset was prepared during office hours, containing 1,633 images across five categories: Banarasi, Bandhani, Handloom, Ikat, and Pichwai.

Dataset Preparation and Preprocessing:
Images were organized into training, validation, and testing sets. The Handloom category was split using an 80:10:10 ratio with a fixed random seed. All images were resized to 224 × 224 pixels and normalized using ImageNet statistics. Training augmentations included random resized cropping, horizontal and vertical flipping, color jitter, and random grayscale conversion. These transformations were applied to improve robustness to changes in color, orientation, and image appearance.

Model Architecture:
A dual-stream architecture was implemented using a pretrained EfficientNet-B0 RGB backbone and a fixed Sobel edge-processing stream. The RGB stream extracts visual features, while the Sobel stream captures edge and contour information related to saree patterns and motifs. GeM pooling was applied to both streams, and their features were fused through a projection head to generate 128-dimensional embeddings. The embeddings were L2-normalized for similarity-based comparison.

Training Strategy:
The model was trained using supervised contrastive loss with a temperature of 0.07. Two augmented views were generated for each image to encourage consistent representations under visual transformations. Balanced P×K sampling was used, selecting five classes and six images per class in each batch. Training used AdamW with a learning rate of 0.0003, weight decay of 0.0001, and cosine annealing over 12 epochs.

Evaluation:
For identification, validation images were used as the gallery and test images as queries. Cosine similarity was used to rank gallery images against each query. The model achieved Rank-1 accuracy of 74.03%, Rank-3 accuracy of 89.61%, Rank-5 accuracy of 96.10%, and mean Average Precision of 65.68%.

For verification, similarity scores were evaluated against category labels. The reported ROC-AUC was 0.7814, with an Equal Error Rate of 31.33%.

Limitation:
The available dataset labels represent saree categories, not unique design identities across different colorways. Therefore, the current results measure category-level similarity rather than confirming whether images show the same design in different colors. Further validation requires design-level labels and multiple color variations of each design.

GitHub: https://github.com/satyananad/assesment
