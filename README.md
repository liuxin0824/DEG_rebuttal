# DEG additional experiments in the ICML rebuttal period

## Section1. Updating the reward diagram.

We further clarify the meanings of different symbols in the schematic diagram to illustrate (i) the differences between the two sub-rewards when applied to the same trajectory, and (ii) how each reward functions across different kinds of trajectories.

![Example Image](image/table2.png)


## Section2. More detailed encoder analysis.

We further provide more results and analysis on the DEG contrastive encoder and pre-trained DINOv3. We first present the performance of DINOv3 on video sequences with different frame intervals. DINOv3 exhibits slightly increased discrimination for frames with larger intervals (i.e., larger semantic distances), but the color distributions remain highly similar, indicating that it cannot adequately map semantic distances to similarity differences. In addition, the average cosine similarity of the first 11 semantically consistent real–generated image pairs encoded by DINOv3 is **0.9384 (far from 1)**, which means it suffers from the noise in generated videos. These disadvantages makes DINOv3 unsuitable for similarity-based reward design. 

![Example Image](image/dinov3.png)

In contrast, the DEG contrastive encoder shows significantly stronger discrimination for frame sequences with larger intervals, while maintaining the ability to align semantically identical frames over a wider temporal range. The first 11 semantically consistent real–generated image pairs encoded by DEG encoder is **0.9952**. Furthermore, for frames that are semantically close but distinct, DEG can effectively map their semantic distance to the similarity difference (heatmap of interval 1), thereby supporting the design of the proposed dual-granularity reward with different thresholds.

![Example Image](image/degencoder.png)

In summary, DEG encoder can better align frames with similar semantics and well map the semantic distances between different frames to their latent distances.

## Section3. DEG is robust to both seen and unseen episodes (initial states).

We visualize the used expert videos and generated RL episodes when faced with both seen and unseen initial states. DEG can well handle ood episodes and generates qualified RL gudiance.

Simulation:
![Example Image](image/sim_videos_comparison.png)

Real-world tasks:
![Example Image](image/real_videos_comparison.png)


## Section4. Hyper-parameters sensitivity experiments.

Regarding the coefficients among different rewards, our goal was to scale them to a similar order of magnitude (range from 10 to 100), which results in good performance. We first change the weighing between coarse-grained reward and fine-grained reward, the results below demonstrate that a similar order of magnitude can better scale these two terms.

![Example Image](image/alphabeta.png)

Then, we change the coefficient of the success sparse reward, as shown below. The larger weights results in better performance, which is consistent with our intuitation, while 10 is enough for effective learning.

![Example Image](image/theta.png)

## Section5. Prompt sensitivity experiments.

In the original paper, we provided as detailed a prompt as possible to maximize generation quality and verify the novel idea of using a large video generation model as an RL guide. In this section, we further conduct fine-tuning with very simple prompts. DEG and DEG+ (DEG with success sparse reward) can still perform effective RL guidance with very brief prompts ‘open the drawer’ and 'push the coffee machine button'.

![Example Image](image/prompt_sensitivity_deg_1_2.png)
![Example Image](image/prompt_sensitivity_deg+_1_2.png)

## Section6. Experiments on harder tasks without success sparse rewards.

DEG still performs better than baselines on harder tasks without success sparse rewards.
| task | DEG | diffusion reward | viper|
|-|-|-|-|
| button-press-topdown    |  **0.60** | 0.33       |0.00|
|assembly|**0.37**| 0.00| 0.00 |

## Section7. Direct comparison between DEG and DEG+.

We include the DEG results in the DEG+ figure and directly compare them. With success sparse reward, our method can perform much better, which is consistent with intuiation.
![Example Image](image/full-comparison.png)




