**Do Modern ImageNet Classifiers Accurately Predict Perceptual Similarity?**
The task of determining the similarity between images is an open problem in computer vision and is crucial for evaluating the realism of machine-generated images. Though there are a number of straightforward methods of estimating image similarity (e.g., low-level metrics that measure pixel differences, such as FSIM and SSIM), in many cases, the measured similarity differences do not match the differences perceived by a person. However, more recent work has demonstrated that intermediate representations of neural network classifiers, such as AlexNet, VGG and SqueezeNet trained on ImageNet, exhibit perceptual similarity as an emergent property. That is, Euclidean distances between encoded representations of images by ImageNet-trained models correlate much better with a person’s judgment of differences between images than estimating perceptual similarity directly from image pixels.


Two sets of sample images from the BAPPS dataset. Trained networks agree more with human judgements as compared to low-level metrics (PSNR, SSIM, FSIM). Image source: Zhang et al. (2018).
In “Do better ImageNet classifiers assess perceptual similarity better?” published in Transactions on Machine Learning Research, we contribute an extensive experimental study on the relationship between the accuracy of ImageNet classifiers and their emergent ability to capture perceptual similarity. To evaluate this emergent ability, we follow previous work in measuring the perceptual scores (PS), which is roughly the correlation between human preferences to that of a model for image similarity on the BAPPS dataset. While prior work studied the first generation of ImageNet classifiers, such as AlexNet, SqueezeNet and VGG, we significantly increase the scope of the analysis incorporating modern classifiers, such as ResNets and Vision Transformers (ViTs), across a wide range of hyper-parameters.

**Relationship Between Accuracy and Perceptual Similarity**
It is well established that features learned via training on ImageNet transfer well to a number of downstream tasks, making ImageNet pre-training a standard recipe. Further, better accuracy on ImageNet usually implies better performance on a diverse set of downstream tasks, such as robustness to common corruptions, out-of-distribution generalization and transfer learning on smaller classification datasets. Contrary to prevailing evidence that suggests models with high validation accuracies on ImageNet are likely to transfer better to other tasks, surprisingly, we find that representations from underfit ImageNet models with modest validation accuracies achieve the best perceptual scores.


Plot of perceptual scores (PS) on the 64 × 64 BAPPS dataset (y-axis) against the ImageNet 64 × 64 validation accuracies (x-axis). Each blue dot represents an ImageNet classifier. Better ImageNet classifiers achieve better PS up to a certain point (dark blue), beyond which improving the accuracy lowers the PS. The best PS are attained by classifiers with moderate accuracy (20.0–40.0).
We study the variation of perceptual scores as a function of neural network hyperparameters: width, depth, number of training steps, weight decay, label smoothing and dropout. For each hyperparameter, there exists an optimal accuracy up to which improving accuracy improves PS. This optimum is fairly low and is attained quite early in the hyperparameter sweep. Beyond this point, improved classifier accuracy corresponds to worse PS.

As illustration, we present the variation of PS with respect to two hyperparameters: training steps in ResNets and width in ViTs. The PS of ResNet-50 and ResNet-200 peak very early at the first few epochs of training. After the peak, PS of better classifiers decrease more drastically. ResNets are trained with a learning rate schedule that causes a stepwise increase in accuracy as a function of training steps. Interestingly, after the peak, they also exhibit a step-wise decrease in PS that matches this step-wise accuracy increase.



Early-stopped ResNets attain the best PS across different depths of 6, 50 and 200.
ViTs consist of a stack of transformer blocks applied to the input image. The width of a ViT model is the number of output neurons of a single transformer block. Increasing its width is an effective way to improve its accuracy. Here, we vary the width of two ViT variants, B/8 and L/4 (i.e., Base and Large ViT models with patch sizes 4 and 8 respectively), and evaluate both the accuracy and PS. Similar to our observations with early-stopped ResNets, narrower ViTs with lower accuracies perform better than the default widths. Surprisingly, the optimal width of ViT-B/8 and ViT-L/4 are 6 and 12% of their default widths. For a more comprehensive list of experiments involving other hyperparameters such as width, depth, number of training steps, weight decay, label smoothing and dropout across both ResNets and ViTs, check out our paper.



Narrow ViTs attain the best PS.
Scaling Down Models Improves Perceptual Scores
Our results prescribe a simple strategy to improve an architecture’s PS: scale down the model to reduce its accuracy until it attains the optimal perceptual score. The table below summarizes the improvements in PS obtained by scaling down each model across every hyperparameter. Except for ViT-L/4, early stopping yields the highest improvement in PS, regardless of architecture. In addition, early stopping is the most efficient strategy as there is no need for an expensive grid search.

Model	Default	Width	Depth	Weight
Decay	Central
Crop	Train
Steps	Best
ResNet-6	69.1	+0.4	-	+0.3	0.0	+0.5	69.6
ResNet-50	68.2	+0.4	-	+0.7	+0.7	+1.5	69.7
ResNet-200	67.6	+0.2	-	+1.3	+1.2	+1.9	69.5
ViT B/8	67.6	+1.1	+1.0	+1.3	+0.9	+1.1	68.9
ViT L/4	67.9	+0.4	+0.4	-0.1	-1.1	+0.5	68.4
Perceptual Score improves by scaling down ImageNet models. Each value denotes the improvement obtained by scaling down a model across a given hyperparameter over the model with default hyperparameters.
**Global Perceptual Functions**
In prior work, the perceptual similarity function was computed using Euclidean distances across the spatial dimensions of the image. This assumes a direct correspondence between pixels, which may not hold for warped, translated or rotated images. Instead, we adopt two perceptual functions that rely on global representations of images, namely the style-loss function from the Neural Style Transfer work that captures stylistic similarity between two images, and a normalized mean pool distance function. The style-loss function compares the inter-channel cross-correlation matrix between two images while the mean pool function compares the spatially averaged global representations.



Global perceptual functions consistently improve PS across both networks trained with default hyperparameters (top) and ResNet-200 as a function of train epochs (bottom).
We probe a number of hypotheses to explain the relationship between accuracy and PS and come away with a few additional insights. For example, the accuracy of models without commonly used skip-connections also inversely correlate with PS, and layers close to the input on average have lower PS as compared to layers close to the output. For further exploration involving distortion sensitivity, ImageNet class granularity, and spatial frequency sensitivity, check out our paper.

**Conclusion**
In this paper, we explore the question of whether improving classification accuracy yields better perceptual metrics. We study the relationship between accuracy and PS on ResNets and ViTs across many different hyperparameters and observe that PS exhibits an inverse-U relationship with accuracy, where accuracy correlates with PS up to a certain point, and then exhibits an inverse-correlation. Finally, in our paper, we discuss in detail a number of explanations for the observed relationship between accuracy and PS, involving skip connections, global similarity functions, distortion sensitivity, layerwise perceptual scores, spatial frequency sensitivity and ImageNet class granularity. While the exact explanation for the observed tradeoff between ImageNet accuracy and perceptual similarity is a mystery, we are excited that our paper opens the door for further research in this area.

Robot learning has been applied to a wide range of challenging real world tasks, including dexterous manipulation, legged locomotion, and grasping. It is less common to see robot learning applied to dynamic, high-acceleration tasks requiring tight-loop human-robot interactions, such as table tennis. There are two complementary properties of the table tennis task that make it interesting for robotic learning research. First, the task requires both speed and precision, which puts significant demands on a learning algorithm. At the same time, the problem is highly-structured (with a fixed, predictable environment) and naturally multi-agent (the robot can play with humans or another robot), making it a desirable testbed to investigate questions about human-robot interaction and reinforcement learning. These properties have led to several research groups developing table tennis research platforms [1, 2, 3, 4].

The Robotics team at Google has built such a platform to study problems that arise from robotic learning in a multi-player, dynamic and interactive setting. In the rest of this post we introduce two projects, Iterative-Sim2Real (to be presented at CoRL 2022) and GoalsEye (IROS 2022), which illustrate the problems we have been investigating so far. Iterative-Sim2Real enables a robot to hold rallies of over 300 hits with a human player, while GoalsEye enables learning goal-conditioned policies that match the precision of amateur humans.

 
Iterative-Sim2Real policies playing cooperatively with humans (top) and a GoalsEye policy returning balls to different locations (bottom).
Iterative-Sim2Real: Leveraging a Simulator to Play Cooperatively with Humans
In this project, the goal for the robot is cooperative in nature: to carry out a rally with a human for as long as possible. Since it would be tedious and time-consuming to train directly against a human player in the real world, we adopt a simulation-based (i.e., sim-to-real) approach. However, because it is difficult to simulate human behavior accurately, applying sim-to-real learning to tasks that require tight, close-loop interaction with a human participant is difficult.

In Iterative-Sim2Real, (i.e., i-S2R), we present a method for learning human behavior models for human-robot interaction tasks, and instantiate it on our robotic table tennis platform. We have built a system that can achieve rallies of up to 340 hits with an amateur human player (shown below).


A 340-hit rally lasting over 4 minutes.
Learning Human Behavior Models: a Chicken and Egg Problem
The central problem in learning accurate human behavior models for robotics is the following: if we do not have a good-enough robot policy to begin with, then we cannot collect high-quality data on how a person might interact with the robot. But without a human behavior model, we cannot obtain robot policies in the first place. An alternative would be to train a robot policy directly in the real world, but this is often slow, cost-prohibitive, and poses safety-related challenges, which are further exacerbated when people are involved. i-S2R, visualized below, is a solution to this chicken and egg problem. It uses a simple model of human behavior as an approximate starting point and alternates between training in simulation and deploying in the real world. In each iteration, both the human behavior model and the policy are refined.


i-S2R Methodology.
Results
To evaluate i-S2R, we repeated the training process five times with five different human opponents and compared it with a baseline approach of ordinary sim-to-real plus fine-tuning (S2R+FT). When aggregated across all players, the i-S2R rally length is higher than S2R+FT by about 9% (below on the left). The histogram of rally lengths for i-S2R and S2R+FT (below on the right) shows that a large fraction of the rallies for S2R+FT are shorter (i.e., less than 5), while i-S2R achieves longer rallies more frequently.


Summary of i-S2R results. Boxplot details: The white circle is the mean, the horizontal line is the median, box bounds are the 25th and 75th percentiles.
We also break down the results based on player type: beginner (40% players), intermediate (40% of players) and advanced (20% players). We see that i-S2R significantly outperforms S2R+FT for both beginner and intermediate players (80% of players).


i-S2R Results by player type.
More details on i-S2R can be found on our preprint, website, and also in the following summary video.


GoalsEye: Learning to Return Balls Precisely on a Physical Robot
While we focused on sim-to-real learning in i-S2R, it is sometimes desirable to learn using only real-world data — closing the sim-to-real gap in this case is unnecessary. Imitation learning (IL) provides a simple and stable approach to learning in the real world, but it requires access to demonstrations and cannot exceed the performance of the teacher. Collecting expert human demonstrations of precise goal-targeting in high speed settings is challenging and sometimes impossible (due to limited precision in human movements). While reinforcement learning (RL) is well-suited to such high-speed, high-precision tasks, it faces a difficult exploration problem (especially at the start), and can be very sample inefficient. In GoalsEye, we demonstrate an approach that combines recent behavior cloning techniques [5, 6] to learn a precise goal-targeting policy, starting from a small, weakly-structured, non-targeting dataset.

Here we consider a different table tennis task with an emphasis on precision. We want the robot to return the ball to an arbitrary goal location on the table, e.g. “hit the back left corner" or ''land the ball just over the net on the right side" (see left video below). Further, we wanted to find a method that can be applied directly on our real world table tennis environment with no simulation involved. We found that the synthesis of two existing imitation learning techniques, Learning from Play (LFP) and Goal-Conditioned Supervised Learning (GCSL), scales to this setting. It is safe and sample efficient enough to train a policy on a physical robot which is as accurate as amateur humans at the task of returning balls to specific goals on the table.

 	
GoalsEye policy aiming at a 20cm diameter goal (left). Human player aiming at the same goal (right).
The essential ingredients of success are:

A minimal, but non-goal-directed “bootstrap” dataset of the robot hitting the ball to overcome an initial difficult exploration problem.
Hindsight relabeled goal conditioned behavioral cloning (GCBC) to train a goal-directed policy to reach any goal in the dataset.
Iterative self-supervised goal reaching. The agent improves continuously by setting random goals and attempting to reach them using the current policy. All attempts are relabeled and added into a continuously expanding training set. This self-practice, in which the robot expands the training data by setting and attempting to reach goals, is repeated iteratively.

**GoalsEye methodology.**
Demonstrations and Self-Improvement Through Practice Are Key
The synthesis of techniques is crucial. The policy’s objective is to return a variety of incoming balls to any location on the opponent’s side of the table. A policy trained on the initial 2,480 demonstrations only accurately reaches within 30 cm of the goal 9% of the time. However, after a policy has self-practiced for ~13,500 attempts, goal-reaching accuracy rises to 43% (below on the right). This improvement is clearly visible as shown in the videos below. Yet if a policy only self-practices, training fails completely in this setting. Interestingly, the number of demonstrations improves the efficiency of subsequent self-practice, albeit with diminishing returns. This indicates that demonstration data and self-practice could be substituted depending on the relative time and cost to gather demonstration data compared with self-practice.


Self-practice substantially improves accuracy. Left: simulated training. Right: real robot training. The demonstration datasets contain ~2,500 episodes, both in simulation and the real world.
 	
Visualizing the benefits of self-practice. Left: policy trained on initial 2,480 demonstrations. Right: policy after an additional 13,500 self-practice attempts.
More details on GoalsEye can be found in the preprint and on our website.

Conclusion and Future Work
We have presented two complementary projects using our robotic table tennis research platform. i-S2R learns RL policies that are able to interact with humans, while GoalsEye demonstrates that learning from real-world unstructured data combined with self-supervised practice is effective for learning goal-conditioned policies in a precise, dynamic setting.

One interesting research direction to pursue on the table tennis platform would be to build a robot “coach” that could adapt its play style according to the skill level of the human participant to keep things challenging and exciting.

