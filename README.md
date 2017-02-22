# Geometric-Methods-in-Data-Analysis
 Circumventing the Disteance Concentration Phenomena
 
Description. Several strategies can be developed to deal with distance concentration phenomena. One of
them, seen in class, consists in using suitable norms, and to project to lower dimensional spaces. Another one,
proposed in [CTP11], consists in using a biasing potential which aims at focusing on the most informative
distances only. In this project, we aim at applying the procedure from [CTP11] to a different molecular data
set, namely an ensemble of conformations of a protein model known as BLN69 [RDRC16]. In a nutshell,
BLN69 is a linear chain of 69 beads; since each bead has 3 cartesian coordinates, a conformation is defined
by a point in dimension d = 3 x 69 = 207. To each conformation, one can also associate an energy, which will
be given along with the conformations. Finally, to measure the distance between two conformations, we shall
use the least root mean square deviation http://sbl.inria.fr/doc/Molecular_distances-user-manual.
html.

N.B on workflow: 
For this project we chose to use the ad-hoc SBL applications provided at http://sbl.inria.fr/applications rather than compiling the entire SBL library which happens to be quite challenging. Therefore we have run several different packages on a VM ubuntu which we combined with some python code to master the given tasks.

#### Question 1 
#### An ensemble of N  10^6 local minima of the BLN69 protein model can be found at http://sbl.
#### inria.fr/data-models. This set is denoted S in the sequel. To get familiar with this data set, select
#### a reasonable number of local minima with low energy, and display them in 2D using multi-dimensional
#### scaling (MDS). For example, you may focus on the 10 lowest local minina. This set is denoted T in
#### the sequel.

In order to generate set T, we import set S and choose the 10 proteins with the lowest associated energies:
```python
# find the index of the 10 lowest minima
idx = np.argpartition(E_S, 10)

# T is the corresponding matrix of coordinates
T = S[idx[0:10],]
```

We run the SBL - Molecular Distance package with the resulting matrix T using the follwing command lines:
```

```

Using the output text file obtained we run a multi-dimentional scaling in sklearn: 
```python
from sklearn import manifold
from sklearn.metrics import euclidean_distances
from adjustText import adjust_text

seed = 1 #generate reproducable results

mds_lrmsd = manifold.MDS(n_components=2, max_iter=3000, eps=1e-9, random_state=seed, 
                         dissimilarity="precomputed", n_jobs=1)

T_dist_mds = mds_lrmsd.fit_transform(T_dist)

fig, ax = plt.subplots()
ax.scatter(T_dist_mds[:, 0], T_dist_mds[:, 1])

texts = []
for i, txt in enumerate(idx[0:10]):
    texts.append(ax.text(T_dist_mds[:, 0][i],T_dist_mds[:, 1][i], txt))
adjust_text(texts)

plt.xticks([-1,-0.5,0,0.5,1])
plt.yticks([-1,-0.5,0,0.5,1])
plt.title("2D MDS on 10 lowest local minima of BNL69")
#plt.show()
plt.savefig('MDS_Q1.png')
```
Here we used a seed in order to be able to compare our results later in the project once we have applied the sketch-map method.
