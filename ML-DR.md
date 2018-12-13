# Machine Learning - Dimension Reduction
By Chen Li
## Introduction
After we obtain an expression matrix RNA-seq, in which every row is a gene, and every column is a sample, how can we get a general sense of the distribution of the data and extract important features (genes) from the data matrix? Note that the expression matrix is usually in a high dimensional space because every gene is a dimension, so directly visualizing the data is impossible for human beings. Dimension reduction techniques such as PCA and t-SNE can map the original data in high dimension to lower dimension while retaining some spatial information (similarity and difference between samples). In this way, we can easily visualize the data in 2D space. In addition, PCA can return the *importances* of the features (genes), which are the ability of these features in grouping and separating the data points. By extracting the most important genes from the raw data matrix, we are able to explore the data such as clustering more easily. 

### PCA
PCA is widely used upstream of calculations that handle high dimensional data badly. PCA is a linear transformation method. It preserves the correlation between point x and y after transformation. PCA can reduce 4 or higher dimension graph to 2D or 3D. Let’s take a expression matrix for 6 mouse samples as an example. We will only use two genes for illustration. (Youtube Reference: [StatQuest](https://www.youtube.com/watch?v=FgakZw6K1QQ))

<img src="https://github.com/danielee0707/BENG183/blob/master/1.png" width="50%" />

Now let's go through the steps in PCA calculation:
1. The data set is first moved so that its center is at origin. Then PCA calculates the top components with the highest variations in the data. What it does is to fit a line to the data set. For an arbitrary line in the plane through the origin, the (sum of squared) distances to the projections of all the points are calculated and maximized. This has the same effect as minimizing all the distances between the points and the line. (In real world, calculation is done by linear algebra.)

<img src="https://github.com/danielee0707/BENG183/blob/master/2.png" width="50%" />
<img src="https://github.com/danielee0707/BENG183/blob/master/3.png" width="50%" />
 
2. This line is called PC1, or **Principal Component** 1, and it captures the largest variation in the data. From the resulting line, we also know what its compositions based on the slope: it contains 4 parts Gene1 plus 1 part Gene2.
    
3. PC2 is simply the line perpendicular to PC1. If the data is in 3D, PC2 will be residing in a plane perpendicular to PC1, and the previous steps will be repeated. All dotted lines are perpendicular to each other.

<img src="https://github.com/danielee0707/BENG183/blob/master/4.png" width="50%" />
<img src="https://github.com/danielee0707/BENG183/blob/master/6.png" width="50%" />

4. Finally, we rotate the coordinate so that PC1 becomes x-axis and PC2 becomes y-axis. The top two PCs are able to explain 94% of all variations in the data.

<img src="https://github.com/danielee0707/BENG183/blob/master/5.png" width="50%" />

**Note:** 
When performing PCA transformation, only the top several PCs are used, and other PCs (dimensions) are discarded. Information is *lost* in this process.

### t-SNE
By projecting data points to a plane of high variability, PCA only preserves the global structure of data points (which means it may not be powerful enough to distinguish subgroups). So we will need t-SNE to see more detailed neighboring structures.

<img src="https://github.com/danielee0707/BENG183/blob/master/7.png" width="50%" />
<img src="https://github.com/danielee0707/BENG183/blob/master/8.png" width="50%" />

[Source](https://www.kaggle.com/puyokw/clustering-in-2-dimension-using-tsne/code)

1. How does tSNE work? The name stands for t-distributed stochastic neighbor embedding. The underlining mathematics of t-SNE is very advance and will not be covered here. But basiclly, it applies neighborhood preserving mapping so that distances between neighboring points are truthfully preserved after transformation.

2. But how do we determine neighbors? *Perplexity* represents roughly the number of potential neighbors considered for a cluster, so we can determine neighbors of each point and thus clusters by trying different perplexity parameters until a reasonable and clear clustering is visualized by tSNE. This is usually determined arbitrarily. (Youtube Reference: [Applied AI Course](https://www.youtube.com/watch?v=FQmCzpKWD48&list=PLupD_xFct8mHqCkuaXmeXhe0ajNDu0mhZ&index=1))

<img src="https://github.com/danielee0707/BENG183/blob/master/9.png" width="80%" />

3. Meanwhile, since there is always some randomality in tSNE’s embedding, we need to run multiple *iterations* to improve the 2D embedding to best represent the original structure. Such number of iterations is another hyperparameter to choose, and generally, the more iterations tSNE runs, the more credible the resulting embedding will be.

<img src="https://github.com/danielee0707/BENG183/blob/master/10.gif" width="50%" />

**Note:**
* The size of each cluster in a t-SNE plot means nothing because it tends to expand dense clusters and shrink sparse ones.
* Since t-SNE only represents distances within a potential cluster, distances between clusters do not provide any information.
