# Name - Ajay Singh
# id - Bitsom_Ba_2511011

# Part 2: CNN Computer Vision - Manufacturing Defect Detection

# dataset
LOCATION = `https://drive.google.com/drive/folders/17xoSIAe-24-18iJiN3zKPqJl-RNDqIeW?usp=drive_link`

## Task 1: Problem Identification

**Problem Type: Image Classification**

This is an image classification problem where each product image must be classified into one of 4 defect categories (Normal, Scratch, Dent, or Stain).

**Why Image Classification and not other types?**
- Object Detection: Not needed - we classify entire product, not locate multiple objects
- Semantic Segmentation: Not needed - we don't need pixel-level boundaries
- Instance Segmentation: Not needed - we classify, not segment individual instances

---

## Task 2: Dataset Exploration

- Total Images: 200 (50 per class)
- Classes: 4 (Normal, Scratch, Dent, Stain)
- Format: PNG (Grayscale)
- Class Distribution: Balanced (25% each class)
- Data Imbalance: None detected

---

## Task 3: Image Preprocessing

1. Loading: Read PNG images in grayscale format
2. Resizing: All images resized to 128×128 pixels
3. Normalization: Pixel values scaled to [0, 1]
4. Train/Test Split: 80% training (160), 20% testing (40) with stratification
5. Data Augmentation: Rotation (±20°), Shift (±20%), Zoom (±20%), Flip

---

## Task 4: CNN Model Creation

Model Architecture:
- Block 1: Conv2D(32) → ReLU → BatchNorm → MaxPool → Dropout
- Block 2: Conv2D(64) → ReLU → BatchNorm → MaxPool → Dropout  
- Block 3: Conv2D(128) → ReLU → BatchNorm → MaxPool → Dropout
- Block 4: Conv2D(256) → ReLU → BatchNorm → MaxPool → Dropout
- Flatten → Dense(512) → Dense(256) → Dense(4, softmax)

Total Parameters: ~1.5 million

---

## Task 5: Model Training and Evaluation

Training Configuration:
- Optimizer: Adam (lr=0.001)
- Loss: Sparse Categorical Crossentropy
- Batch Size: 32
- Epochs: 50 (Early Stopping, patience=10)

Output Files:
1. results/accuracy_loss_curves.png (training/validation metrics)
2. results/confusion_matrix.png (classification heatmap)
3. sample_predictions/prediction_outputs.png (9 predictions)

---

## Task 6: CNN Concept Explanation

**What is Convolution?**
Convolution applies a filter (kernel) over an image to extract features like edges, textures, and corners. The filter slides across the image and multiplies pixel values by weights.

**Why is Pooling Used?**
Pooling reduces feature map dimensions by extracting maximum values in small windows. It reduces computation, extracts important features, and provides translation invariance.

**Why is ReLU Commonly Used?**
ReLU (f(x) = max(0, x)) is computationally efficient, prevents vanishing gradients, and introduces non-linearity for learning complex patterns better than sigmoid/tanh.

**Why are CNNs Better than Feed-Forward Networks?**
1. Spatial Structure: CNNs preserve 2D relationships; feed-forward flattens them
2. Parameter Sharing: CNNs reuse filters (1.5M params); feed-forward needs 268M params
3. Local Connectivity: CNNs connect to small regions; feed-forward connects to all
4. Translation Invariance: CNNs recognize objects at any position

CNNs are 178× more efficient for 128×128 images!

---

## Task 7: Business Use Case Mapping

**Domain: Manufacturing Quality Inspection**

**Problem:** Manual defect inspection is slow (5-10 min/item), inconsistent, error-prone (10-15% miss rate), and expensive.

**Solution:** CNN-based automated defect detection on production lines.

**Workflow:**
- Camera captures product image
- CNN classifies as: Normal/Scratch/Dent/Stain
- Automatic Accept/Reject decision
- Real-time segregation of defective items

**Benefits:**
- Speed: 50-100ms per image (vs 5-10 minutes manual)
- Accuracy: 90%+ detection rate
- Cost: Saves ~$75K/year
- Scalability: Process 10,000+ items/day vs 100 manually

**Other Applications:** Healthcare (medical imaging), Agriculture (crop disease), Retail (quality checks), Autonomous vehicles, Security surveillance

### Deployment Scenarios
- Real-time camera feeds in production lines
- Post-production quality checkpoints
- Automated sorting systems
- Mobile inspection tools

## Key Insights

### Why CNNs for Image Classification?
1. **Spatial Locality:** Captures local patterns in images
2. **Translation Invariance:** Recognizes features regardless of position
3. **Parameter Sharing:** Reduces parameters vs fully connected networks
4. **Hierarchical Learning:** Learns progressively complex features

### Model Strengths
- Automated feature extraction
- No manual feature engineering needed
- Good generalization with data augmentation
- Interpretable through feature maps and attention

### Potential Improvements
- **Transfer Learning:** Use pretrained models (ResNet, MobileNet, EfficientNet)
- **Ensemble Methods:** Combine multiple models for higher accuracy
- **Uncertainty Quantification:** Estimate confidence in predictions
- **Fine-tuning:** Retrain on production data for domain adaptation
- **Multi-scale Processing:** Process images at different resolutions
