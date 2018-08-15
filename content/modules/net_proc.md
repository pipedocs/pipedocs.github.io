[Return to modules >](https://pipedocs.github.io//modules)

# `net`: Network analysis module

`net` performs basic analysis of networks previously computed in other modules such as `fcon`. `net` operates on adjacency matrices, not images; adjacency matrices must be computed before `net` can be run. At this time, `net` principally computes community-related measures, beginning with computation of the community partition. A standard community detection algorithm is applied in order to identify major subnetworks or modules within the overall network. Community detection requires MATLAB; even if MATLAB is not available and community detection is not performed, community-based statistics will still be computed using an _a priori_ community partition if one is available for the current network. Afterward, within- and between-community connectivity measures are computed for potential use in group-level analyses.

### `net_atlas`

_Brain atlas or parcellation._

Contains a comma-separated list of the names of the atlases over which network measures should be computed. The atlases should correspond to valid paths in `$XCPEDIR/atlas` or another appropriate `$BRAINATLAS` directory. All atlases listed here must be run through the `fcon` module before they can be used here.

```bash
# Use the Power 264-sphere parcellation only
net_atlas[cxt]=power264

# Use both the Power 264 atlas and the Gordon atlas
net_atlas[cxt]=power264,gordon

# Use the 400-node version of the Schaefer atlas
net_atlas[cxt]=schaefer400

# Use all available resolutions of the Schaefer atlas
net_atlas[cxt]=schaefer

# Use all available atlases
net_atlas[cxt]=all
```

### `net_gamma`

_Resolution parameter for community detection._

The resolution parameter gamma reflects the size and number of communities detected by the Louvain-like algorithm. A gamma value of 1 represents the default maximisation of modularity criterion. Gamma values greater than 1 favour a greater number of smaller communities (higher resolution), whereas gamma values less than one favour fewer, larger communities (lower resolution). To perform community detection at multiple resolutions, enter a comma-separated list of gamma values.

```bash
# Resolution parameter of 1
net_gamma[cxt]=1

# Resolution parameters of 1 and 7
net_gamma[cxt]=1,7

# All resolution parameters from 1 to 7 in increments of 1
net_gamma[cxt]=1:7

# All resolution parameters from 1 to 7 in increments of 0.1
net_gamma[cxt]=1:0.1:7
```

### `net_consensus`

_Minimum convergent iterations for consensus._

Many community detection algorithms are degenerate; i.e., they have multiple correct solutions. A consensus partition is the result of sampling the solution space repeatedly and selecting a partition based on the number of times that each pair of nodes is classified into the same community. This is determined by generating a probabilistic 'agreement matrix' in which the weight of each edge encodes the probability that two nodes are classified into the same community over all sampled solutions. A higher number of iterations will result in a sample that is more representative of the full solution space. After the agreement matrix is generated, community detection is repeated using the agreement matrix instead of the adjacency matrix, again sampling the solution space the same number of times to produce a revised agreement matrix. When the solution space, as reflected in the agreement matrix, is deterministic, then the algorithm has converged on a consensus partition.

```bash
# 100 convergent iterations required for consensus
net_consensus[cxt]=100

# No consensus -- use only one partition
net_consensus[cxt]=1
```

### `net_com`

_Community detection algorithm._

*(requires MATLAB)*

The generalised Louvain-like algorithm (Mucha et al., 2010) partitions a graph into subgraphs, or 'communities', by merging nodes so as to maximise the value of the modularity coefficient, which can be intuitively understood in terms of the number of within-community edges for a putative partition of the graph as compared with the number of within-community edges for the same partition of a random graph with similar properties (degree, node and edge count). Unlike the original Louvain method (Blondel et al., 2008), the generalisation operates on a modularity matrix instead of the adjacency matrix and supports multislice or multiplex networks, such as those in connectivity dynamics.

```bash
# Use the generalised Louvain-like algorithm
net_com[cxt]=genlouvain
```

### `net_rerun`

Ordinarily, each module will detect whether a particular analysis has run to completion before beginning it. If re-running is disabled, then the module will immediately skip to the next stage of analysis. Otherwise, any completed analyses will be repeated.If you change the run parameters, you should rerun any modules downstream of the change.

```bash
# Skip processing steps if the pipeline detects the expected output
net_rerun[cxt]=0

# Repeat all processing steps
net_rerun[cxt]=1
```

### `net_cleanup`

Modules often produce numerous intermediate temporary files and images during the course of an analysis. In many cases, these temporary files are undesirable and unnecessarily consume disk space. If cleanup is enabled, any files stamped as temporary will be deleted when a module successfully runs to completion. If a module fails to detect the output that it expects, then temporary files will be retained to facilitate error diagnosis.

```bash
# Remove temporary files
net_cleanup[cxt]=1

# Retain temporary files
net_cleanup[cxt]=0
```

[Return to modules >](https://pipedocs.github.io//modules)
