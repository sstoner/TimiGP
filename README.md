# TimiGP

[![DOI](https://zenodo.org/badge/467312934.svg)](https://zenodo.org/badge/latestdoi/467312934)

An `R package` to infer cell-cell interactions and clinical values in tumor immune microenvironment through gene pairs.

For more details, please read our manuscript: [TimiGP: Inferring cell-cell interactions and prognostic associations in the tumor immune microenvironment through gene pairs.](https://doi.org/10.1016/j.xcrm.2023.101121)

## Prognosis module
### Background 
The immune cell co-infiltration sometimes causes `Prognostic Bias`, which makes existing transcriptome-based cell-type quantification methods challenging to identify the biological and clinical values. The below figure demonstrates the prognostic bias in metastatic melanoma: almost all cell types inferred by 8 popular methods are associated with favorable prognosis

![Fig1A](/assets/images/Fig1A.png)

### Rationale

As the representative schema shown above, the absolute infiltration of immune effectors and suppressors are positively correlated and therefore have the same impact on prognosis. One solution is to consider the pairwise relation between cells. For example, relative abundance enables us to capture subtle differences between them and reveal the prognostic values of those cells in line with their biological functions(left). 

Based on this analysis and the characteristics of the immune system, we propose a novel concept, `Clinically relevant cell-cell interaction`, to investigate Tumor Immune Microenvironment(TIME) by Gene Pairs(right). The tumor immune microenvironment(TIME) is a balance between anti-tumor and pro-tumor immune cells. If the function of anti-tumor cell types is more vital than the pro-tumor cells (e.g., higher abundance, higher marker expression), the TIME is associated with favorable patients’ prognosis; otherwise, it is associated with unfavorable patients’ prognosis.

![Fig1E](/assets/images/Fig1E_S11.png)

### TimiGP framework
Based on the rationale, we developed a novel method, `TimiGP, Tumor Immune Microenvironment Illustration based on Gene Pairing`. Below is the **Overview of TimiGP framework**.

![Overview of TimiGP framework](/assets/images/Fig2.png)

`Inputs`
 1. Immune cell-type markers; 
 2. Transcriptomic profiles; 
 3. Survival statistics, including events (e.g., death, recurrence) and time-to-event of the same cohort. 

`TimiGP Steps`
 1. Defining and selecting marker gene pair matrix;
 2. Choosing gene pairs associated with favorable prognosis; 
 3. Constructing the directed gene-gene network; 
 4. Identifying cell-cell interactions by enrichment analysis;
 5. Building and analyzing cell interaction networks.
 
`Outputs`
 1. Cell-cell interaction network,
 2. Prognostic association of infiltrating cells (favorability score).
 
### TimiGP Applications
TimiGP is designed to **infer the cell-cell interaction network and prognostic association of immune cells**. Based on the resulting immunological insights, The method will **facilitate the development of prognostic models**. Taking advantage of different biomarker references derived from **bulk and single-cell RNA-seq**, TimiGP can be applied to investigate the **entire tumor microenvironment** or **cell subpopulations**, perform **pan-cancer analysis** or study **other diseases**. 

 ![FigS12](/assets/images/FigS12.png)
 
## Citation
This package is intended for research use only. 

If you use TimiGP in your publication, please cite the paper:

Li, C. et al. TimiGP: Inferring cell-cell interactions and prognostic associations in the tumor immune microenvironment through gene pairs. Cell Reports Medicine. (2023).

### Relavant publications
1. Cell Reports Medicine: [TimiGP: Inferring cell-cell interactions and prognostic associations in the tumor immune microenvironment through gene pairs](https://doi.org/10.1016/j.xcrm.2023.101121)
1. AACR 2023 abstract: [TimiGP: Dissect the tumor immune microenvironment and its association with survival and immunotherapy response](https://aacrjournals.org/cancerres/article/83/7_Supplement/2080/723394/Abstract-2080-TimiGP-Dissect-the-tumor-immune)
2. bioRxiv preprint: [TimiGP: inferring cell-cell functional interactions and clinical values in the tumor immune microenvironment through gene pairs.](https://www.biorxiv.org/content/10.1101/2022.11.17.515465v1.full)

## TimiGP system To-do-list

- [x] TimiGP concept: [prognosis module](https://doi.org/10.1016/j.xcrm.2023.101121) and R package (v1.1.0)
- [x] Code optimization (v1.2.0) and publish the protocol for the prognosis module (Under Revision)
- [x] TimiGP analysis: response module (wrap-up)
- [x] TimiGP tool: **single sample module** (validated)
- [ ] Publish preprint and package for **response module** (Expected: Oct 2023)
- [ ] Publish peer-reviewed paper of **response module** 
- [ ] TimiGP upgrade: **single cell module** (conceptualized)
- [ ] TimiGP WebApp

## Contributing

We greatly welcome contributions to TimiGP. Please submit a pull request if you have any ideas (e.g., new modules and functions) or bug fixes. We also welcome any issues you encounter while using TimiGP.

## Installation
 1. Install [the `devtools` package](https://github.com/r-lib/devtools) from CRAN.
 2. Run `devtools::install_github("CSkylarL/TimiGP")`.
 
 Follow the below code:
```R
# Install devtools from CRAN.
install.packages("devtools")

# Install TimiGP from GitHub.
devtools::install_github("CSkylarL/TimiGP")

# Load the package
library(TimiGP)
```
## Usage of prognosis module
Here is a summary of the functions in the package:
| Step                                                               | Function                                                        |
| ------------------------------------------------------------------ | --------------------------------------------------------------- |
| 1.1   Preprocess of Clinical Info                                  | `TimiCheckEvent`(info = NULL) |  
| 1.2   Preprocess of Transcriptome Profile                          | `TimiPrePropress`(marker, rna = NULL, cohort, log = TRUE, GMNorm=TRUE)|
| 2.1   Pairwise Comparison                                          | `TimiGenePair`(rna = NULL, cont = FALSE)|
| 2.2   Prognostic IMGP Selection with Cox regression              | `TimiCOX`(mps = NULL,info = NULL, p.adj = "BH")|
| 2.3   Directed Gene Network                                        | `TimiGeneNetwork`(resdata = NULL, select = NULL, dataset = NULL, geneset = NULL, condition = "QV", cutoff = 0.05, export = TRUE, path = NULL)|
| 3.1   Cell Interaction Annotation                                  | `TimiCellPair`(geneset = NULL, dataset = NULL, core = 1)|
| 3.2   Prepare Enrichment Background                                | `TimiBG`(marker.pair = NULL)|
| 3.3.1 Enrichment Analysis                                          | `TimiEnrich`(gene = NULL, background = NULL, geneset = NULL, p.adj = NULL, core = 1, pair = TRUE)|
| 3.3.2 Permutation Test                                             | `TimiPermFDR`(resdata = NULL, geneset = NULL, gene = NULL, background = NULL, niter = 100, core = 1)|
| 3.3.3 Visualization: Dotplot of Cell Interaction                   | `TimiDotplot`(resdata = NULL, select = 1:5)|
| 3.4.1 Visualization: Chord Diagram of cell-cell Interaction       | `TimiCellChord`(resdata = NULL,select = NULL, dataset = NULL,group = NULL, color = NULL, condition = "Adjust.P.Value", cutoff = 0.05)|
| 3.4.2 Visualization: Chord Diagram of Selected Gene Interaction    | `TimiGeneChord`(resdata = NULL, select = 1, color = NULL)|
| 3.5.1 Functional Interaction Network                               | `TimiCellNetwork`(resdata = NULL, select = NULL, dataset = NULL, group = NULL, geneset = NULL, condition = "Adjust.P.Value", cutoff = 0.05, export = TRUE, path = NULL)|
| 3.5.2 Favorability Score                                           | `TimiFS`(resdata = NULL, condition = "Adjust.P.Value", cutoff = 0.05)|
| 3.5.3 Visualization: Bar plot of Favorability Score                | `TimiFSBar`(score = NULL, select = NULL)|

## Available Data
Here is a summary of available data in the package:
| Available Data                                             | Description                                                       |
| ---------------------------------------------------------- | ------------------------------------------------------------------| 
| 1.1   data(SKCM06info)                                     | TCGA SKCM06(metastatic melanoma) clinical info                     |
| 1.2   data(SKCM06rna)                                      | TCGA SKCM06(metastatic melanoma) transcriptome profile             |
| 2.1   data(Immune_Marker_n1293)                            | #1293 immune cell markers                                         |
| 2.2   data(`CellType_Charoentong2017_Bindea2013_Xu2018_Immune`)| Immune cell types and markers from 3 publications             |
| 3.1   data(`CellType_Tirosh2016_melanoma_TME`)             | Metastatic melanoma tumor microenvironment cell types and markers |
| 4.1   data(`CellType_Zheng2021_Tcell`)                     | T cell subtypes and markers                                       |
| 5.1   data(`CellType_Bindea2013_cancer`)                   | Markers of immune cell types and 1 cancer cell type               |
| 5.2   data(Bindea2013c_COX_MP_SKCM06)                      | `TimiCOX` results with the above annotation that reveals the association between each marker pair and favorable prognosis.   |
| 5.3   data(Bindea2013c_enrich)                             | An example of `TimiEnrich` and `TimiPermFDR` results on cell interactions           |
| 6.1   data(`CellType_Newman2015_LM22`)                     | Markers of immune cell types(LM22 signature) generated by CIBERSORT|

Here is an example[01](example/example01_Bindea2013_Cancer.R) of how to use the package to infer gene and cell interaction based on relative abundance. 
Other examples can be found in the [example](example/) folder. And the process to generate the cell markers can be found in the [inst/extdata](inst/extdata) folder.

Library the package
```R
library(TimiGP)
rm(list=ls())
```

### 1.   Preprocess of clinical information and transcriptome profile
`TimiCheckEvent` will filter out the low-quality clinical info(event and time-to-event).

`TimiPrePropress` will extract the transcription profile of selected markers and cohorts, then perform log transformation and gene-wise median normalization.
```R
rm(list=ls())
#1. Load SCKCM06 data ----
data("SKCM06info")
head(SKCM06info)
data("SKCM06rna")
#2. Load cell type and marker annotation ----
data("CellType_Bindea2013_cancer")
geneset <- CellType_Bindea2013_cancer
marker <- unique(geneset$Gene)
#3. Preprocess: TimiCheckEvent & TimiPrePropress ----
info <- TimiCheckEvent(SKCM06info)
rna <- TimiPrePropress(marker = marker,rna = SKCM06rna,cohort = rownames(info))
```
### 2 Gene Interaction
#### 2.1   Pairwise Comparison
In default, `TimiGenePair` will capture the logical relation of any two marker pairs, and generate a matrix of Marker Pair Score(MPS):

  - 1 or TRUE = the expression of gene A > that of gene B, 

  - 0 or FALSE = the expression of gene A < that of gene B.

Optional:  Capture the continuous relation of any two marker pairs by setting `cont = T`, and generate a matrix of Marker Pair Scores:

  -  the expression of gene A - that of gene B (See [example06_Bindea2013_Cancer_continuous_pair.R](example/example06_Bindea2013_Cancer_continuous_pair.R))
```R
#4. Generate marker pair score: TimiGenePair  ----
mps <- TimiGenePair(rna)
```
#### 2.2   Directed IMGP Selection
`TimiCOX` will perform univariate Cox regression that fits each marker pair as a variable. The result of Cox regression is returned as the first list. If a Pair A_B is associated with a poor prognosis(HR > 1), even if not significant, it will be changed to B_A and reverse its value in the matrix of the pair. The new matrix of Marker Pair Score(MPS) is returned as the second list.


This step takes about 5-10 min, which depends on the number of gene pairs.

```R
#5. Perform univariate Cox regression to find the association between marker pair and survival: TimiCOX ----
res <- TimiCOX(mps = mps,info = info,p.adj = "BH")
mps <- res[[1]]
cox_res <- res[[2]]

# This step takes about 20-30 min, the result has been saved in data as examples
# Bindea2013c_COX_MP_SKCM06 <- cox_res
# save(Bindea2013c_COX_MP_SKCM06, file = "data/Bindea2013c_COX_MP_SKCM06.rda")
```
#### 2.3   Directed Gene Network
By setting "export = TRUE", `TimiGeneNetwork` generates three files that can be used to build a network in Cytoscape: 
 - network files: simple interaction file (gene_network.sif); 
 - node attributes (gene_node.txt); 
 - edge attributes (gene_edge.txt). 
The function also returns a list of the above files that can be modified in R.
```R
rm(list=ls())
# 6. Generate Directed Gene Network: TimiGeneNetwork  ----
data(Bindea2013c_COX_MP_SKCM06)
cox_res <- Bindea2013c_COX_MP_SKCM06
# You can use Cytoscape to visualize the network by setting "export = TRUE"
NET <- TimiGeneNetwork(resdata = cox_res,dataset = "Bindea2013_Cancer",export =TRUE, path = "./")
```
### 3 Cell Interaction
`TimiCellPair` will generate marker pair annotations of cell interactions. For example, given any two different cell types, Cell A has markers a1 and a2 and Cell B has markers b1 and b2. Cell interaction A_B includes marker pairs: a1_b1, a1_b2, a2_b1, a2_b2 while cell interaction B_A includes marker pairs: b1_a1, b1_a2, b2_a1, b2_a2.
#### 3.1   Cell Interaction Annotation
```R
rm(list=ls())
# 7. Generate Cell Interaction Annotation: TimiCellPair ----
data(CellType_Bindea2013_cancer)
geneset <- CellType_Bindea2013_cancer
cell_pair <- TimiCellPair(geneset = geneset,core = 20)
```
#### 3.2   Prepare Enrichment Background
`TimiBG` will generate background gene pairs for enrichment analysis. The background should include both directions of each pair. For example, given a gene pair A_B, it will generate the background gene pairs A_B and B_A.
```R
# 8. Generate background: TimiBG ----
background <- TimiBG(marker.pair = row.names(cox_res))
```
#### 3.3   Enrichment Analysis
`TimiEnrich` will perform the analysis according to the query gene pairs. 
```R
# 9. Query: Select marker pairs A_B=1 significantly associated with good prognosis ----
data(Bindea2013c_COX_MP_SKCM06)
cox_res <- Bindea2013c_COX_MP_SKCM06
GP <- rownames(cox_res)[which(cox_res$QV<0.05)]
# 10. Enrichment Analysis: TimiEnrich ----
res <- TimiEnrich(gene = GP, background = background, 
                  geneset = cell_pair, p.adj = "BH",core=20)
                  
# Optional: Permutation to generate FDR: TimiPermFFDR ----
res <- TimiPermFDR(resdata = res, geneset = geneset, gene = GP,
                   background = background, niter = 100, core = 20)
# This step takes some time. Please be patient

# This has been saved to data as an example
# Bindea2013c_enrich <- res
# save(Bindea2013c_enrich,file = "data/Bindea2013c_enrich.rda")
```
`TimiDotplot` can visualize the enrichment analysis results.
```R
# 11. Visualization: Dot plot of selected cell interaction: TimiDotplot-----
rm(list=ls())
data("Bindea2013c_enrich")
res <- Bindea2013c_enrich
p <- TimiDotplot(resdata = res,select = c(1:10))
p
```
![Fig3A](/assets/images/Fig3A.png)

`TimiCellChord` can visualize the cell interaction in Chord Diagram.
```R
# 12. Visualization: Chord Diagram of functional interaction: TimiCellChord----
rm(list=ls())
data("Bindea2013c_enrich")
res <- Bindea2013c_enrich
# Cell Chord Diagram
TimiCellChord(resdata = res,dataset = "Bindea2013_Cancer")
```
![Fig3B](/assets/images/Fig3B.png)

If you are interested in enriched marker pairs in specific cell interactions such as Cytotoxic cells_Cancer cells, please use `TimiGeneChord`.
```R
# Chord Diagram of marker pairs in select cell interaction
TimiGeneChord(resdata = res,select = 3)
```
![Fig3x](/assets/images/Fig3x.png)
#### 3.4  Cell Interaction Network
By setting "export = TRUE", `TimiCellNetwork` generates three files that can be used to build the network in Cytoscape: 
 - network files: simple interaction file (cell_network.sif); 
 - node attributes (cell_node.txt); 
 - edge attributes (cell_edge.txt). 
The function also returns a list of the above files that can be modified in R.

```R
# 13. Generate Directed Cell Network: TimiCellNetwork  ----
# You can use Cytoscape to visualize the network
rm(list=ls())
data("Bindea2013c_enrich")
res <- Bindea2013c_enrich
NET <- TimiCellNetwork(resdata = res,dataset = "Bindea2013_Cancer",export =TRUE, path = "./")
```
![Fig3C](/assets/images/Fig3C.png)
#### 3.5  Favorability Score
Based on the degree information of the cell interaction network, `TimiFS` calculates the favorability score of each cell type. In the network, 

 - the out-degree of cell A means how many times high A-over-other cell function is associated with favorable prognosis; 

 - the in-degree of cell A means how many times low A-over-other cell function is associated with favorable prognosis. 

The favorability score includes a favorable score and an unfavorable score. 
 - Favorable score: out-degree of the cell/sum of out-degree of all cell*100. 
 - Unfavorable score: in-degree of the cell/sum of in-degree of all cell*100.


```R
# 13. Calculate favorability score: TimiFS ----
# Visualization: TimiFSBar 
rm(list=ls())
data("Bindea2013c_enrich")
res <- Bindea2013c_enrich
# Calculate
score <- TimiFS(res)
head(score)
```
This result can be visualized by `TimiFSBar`.
```R
# Visualization
p <- TimiFSBar(score,select = c(1:5,(nrow(score)-2):nrow(score)))
p
```
![Fig3D](/assets/images/Fig3D.png)
