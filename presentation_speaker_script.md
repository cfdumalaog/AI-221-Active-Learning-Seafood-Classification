# Speaker Script

Target length: about 8.5 to 9 minutes  
Style: explain the story, do not read every bullet

## Slide 1. Title
Time: 20 seconds

Good day everyone. We are Crizepvill Dumalaog and Francis Neil Jarical, and our project is titled **Cutting Labeling Costs in an Imbalanced Seafood Dataset: An Active Learning Approach with ResNet-18**.

The main question is simple: if labels are expensive, can we train a useful seafood classifier by asking humans to label only the most informative samples?

## Slide 2. Introduction and Active Learning
Time: 1 minute 20 seconds

Seafood classification is a practical computer vision problem. If classification can be automated, it can support biodiversity monitoring, illegal fishing detection, and industrial seafood sorting. The challenge is that deep learning models usually need many labeled examples, and marine labels may require domain expertise.

That is where active learning becomes useful. Instead of labeling everything, the model starts with a small labeled set, scans the unlabeled pool, and asks the annotator to label only the examples it finds most informative.

In our project, the query strategy is **uncertainty sampling**, specifically the **Least Confidence** metric. In plain terms, we choose the images where the model is least confident about its top prediction. Our hypothesis is that this should be more label-efficient than randomly choosing images to label.

## Slide 3. Dataset and Preprocessing
Time: 1 minute 10 seconds

We used **A Large Scale Fish Dataset**, which contains 9 seafood classes with 1000 images per class. The classes include Red Sea Bream, Gilt-Head Bream, Sea Bass, Red Mullet, Striped Red Mullet, Horse Mackerel, Black Sea Sprat, Trout, and Shrimp.

For the fixed evaluation splits, we used 200 images per class for testing and 100 images per class for validation. We started with only 10 labeled training images per class, so the initial labeled pool had just 90 images.

The remaining training images formed the unlabeled pool. To make the task more realistic, we throttled that pool into a long-tailed distribution, where some classes remained common while others became rare.

## Slide 4. Model and Experimental Setup
Time: 1 minute 10 seconds

For the classifier, we used **ResNet-18**. This was practical because active learning requires repeated retraining and repeated inference over the unlabeled pool. ResNet-18 is lightweight enough for that loop, but still strong enough to extract useful visual features.

The model was initialized with ImageNet weights, and the final layer was replaced to output 9 seafood classes. Training used Adam, cross-entropy loss, a StepLR scheduler, and early stopping based on validation loss.

The active learning loop used a strict budget: 10 queried samples per iteration, for 10 iterations. So after starting with 90 labeled images, the model could add only 100 more labels. We compared this against a random sampling baseline under the same budget.

To reduce randomness, we repeated the experiments across three seeds: 5, 42, and 101. The reported curves and table values are averages across those runs.

## Slide 5. Supervised Learning Baseline
Time: 45 seconds

Before evaluating active learning, we trained a fully supervised ResNet-18 using the entire training pool. This gives us an upper-bound reference: what happens if labeling cost is not a constraint?

The fully supervised model achieved a test accuracy of **1.0000** and a test Macro F1 of **1.0000**. So the dataset is highly learnable when all labels are available.

But that is the point: full supervision works well, but it requires thousands of labels. Our active learning experiment asks how close we can get with only a small fraction of the training pool.

## Slide 6. Active Learning vs Random Sampling
Time: 1 minute

This slide shows the main result. Both active learning and random sampling improve as the labeled pool grows, but active learning shows a clearer upward trend.

The strongest result appears around iteration 8. At that point, active learning reached a mean Test Macro F1 of **0.8107**, while random sampling reached only **0.7371**.

This is important because Macro F1 treats every class more equally. Since our training pool is imbalanced, accuracy alone could hide poor performance on rare classes. Macro F1 is the better metric for asking whether the model generalizes across all seafood categories.

The interpretation is that Least Confidence sampling adds data from regions where the model is confused, which tends to be more valuable than random images.

## Slide 7. Confusion Matrices
Time: 40 seconds

The confusion matrices help us understand the errors behind the numbers.

For the active learning model, one visible error pattern is confusion between **Red Sea Bream** and **Gilt-Head Bream**. That makes sense because these classes are visually similar.

For the random sampling model, the notable confusion is **Striped Red Mullet** being classified as **Shrimp**. This suggests that random sampling may fail to give enough useful examples for some minority or difficult classes.

The key point is not that active learning removes all confusion. It appears to spend the labeling budget on harder boundary cases, which improves class-level balance.

## Slide 8. Performance Table
Time: 45 seconds

This table gives the numerical details behind the learning curve. I will not read every row, but I want to highlight two points.

First, the labeled budget is extremely small. Even at iteration 8, we are using only **2.70%** of the available training data.

Second, at that same budget, active learning achieved **0.8107 Test Macro F1**, compared with **0.7371** for random sampling. With the same annotation budget, uncertainty sampling produced a stronger classifier.

## Slide 9. Unsupervised Feature Analysis
Time: 50 seconds

To understand why active learning helps, we also looked at t-SNE projections of the learned feature embeddings.

At the beginning, the feature space is still highly entangled. Classes overlap because the model has seen very few labeled examples. But as the active learning loop progresses, the feature space becomes more separated.

The interesting part is where the queried samples appear. Random sampling tends to pick points throughout the space, including redundant points inside already dense clusters. Least Confidence sampling tends to pick points near boundaries, where classes overlap and the model is unsure.

So the t-SNE result supports the quantitative result: active learning refines the decision boundaries more efficiently.

## Slide 10. Wrapping Up
Time: 1 minute

To summarize, this project shows that active learning can reduce labeling effort in an imbalanced seafood classification setting. Starting from only 90 labeled images, and adding 10 queried samples per iteration, the active learning model reached a Test Macro F1 of **0.8107** using only **2.70%** of the available training data.

Compared with full supervision, this represents an approximate **97.3% reduction in labeling effort**, while still reaching over four-fifths of the supervised baseline’s Macro F1 performance.

There are still limitations. We used only Least Confidence sampling, which looks only at the highest predicted class probability. Other acquisition functions, such as entropy, margin sampling, or diversity-aware strategies, may perform better. The imbalance was also synthetic, so future work should test this on naturally imbalanced marine datasets with real label noise.

Overall, our conclusion is that active learning is a promising way to make marine computer vision more label-efficient. It does not remove the need for expert annotation, but it helps spend that expert effort more intelligently.

## Optional Closing Line
Time: 5 seconds

Thank you, and we are happy to answer any questions.
