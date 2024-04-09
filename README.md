# Problem Set 6: Reproducible, shareable code
## Anna Lapteva  $\text{ }\mid\mid\text{ }$   April 5, 2024

### Overview.
This repository contains the code, data, and figures for the project titled "Comorbidity and exclusion of neurological disorders predicted by human gastrointestinal microbiome compositions". As of April 5, 2024, the repository includes the necessary materials to generate SVGs of two reduced dimension plots of microbial relative abundance data for a set of six neurological/neurodevelopmental disorders (Austism Spectrum Disorder (ASD), Alzheimer's Disease (AD), Parkinson's Disease (PD), Multiple Sclerosis (MS), and epilepsy). The dimension reduction algorithm used is Uniform Manifold Approximation and Projection (UMAP), which is appropriate for visualization and general non-linear dimension reduction. The documentation used for implementing UMAP in Python can be found [here](https://umap-learn.readthedocs.io/en/latest/index.html).

### Data.
The data used in this project comes from the [Data Repository for Human Gut Microbiota (GMRepo)](https://gmrepo.humangut.info/home). The paper describing this database can be found [here](https://academic.oup.com/nar/article/50/D1/D777/6426060?login=true). The repository contains metadata manually curated from 71.642 human gut runs/samples from 353 studies/projects. We are interested in sequencing data from human samples, which can tell us about the distribution of genera in the gut microbiome across distinct phenotypes. The advantage of using this curated dataset is that it is annotated and enables grouping by associated human phenotypes. However, the database does not allow for the download of all run data together for a particular phenotype. Thus, the following workflow was used to gather the .txt files containing relative abundance of microbial genera. 

For each phenotype:
1. Look up phenotype of interest in the search bar at the top of the [GMRepo home page](https://gmrepo.humangut.info/home).
2. Navigate to "Related samples/runs" and click "Download all runs as tsv"
3. Open the TSV file and add a column containing the link to each run. This can be achieved by the formula "=CONCAT("https://gmrepo.humangut.info/data/run/", run ID cell number)". The resulting link for run ID ERR1072832 should thus be https://gmrepo.humangut.info/data/run/ERR1072832.
4. Navigate to the QC Status column in the TSV file -- it should be the last column. Filter it to only show entries with QC Status = 1.
5. Copy all links for QC Status = 1 and open them as individual tabs in a new window. This can likely be done in many ways, but I used the [Copy All URLs](https://chromewebstore.google.com/detail/copy-all-urls/djdmadneanknadilpjiknlnanaolmbfk) extension on Google Chrome to open all copied links simultaneously.
6. For each tab, which corresponds to a single run (i.e. one individual's relative abundance data), click "Download relative genus abundance data as tsv". Close the tab and move onto the next tab.
7. Move all downloaded files into the appropriate folder (e.g. all ASD files are moved into ASD folder).

The above process was repeated for all disorders. 

Examining the downloaded .txt files, we can see that the file begins with some contextual information before providing the relative abundance data. The data is presented in three columns corresponding to NCBI Taxonomy ID, Relative Abundance, and Scientific Name. The purpose of the code in "20440_project_data_cleanup.ipynb" is to take the data in these columns, assign the diagnosis (e.g. AD, MS, etc.), and concatenate it into one large dataset that is suitable for use in performing UMAP dimension reduction.

### Folder Structure and Files.
This repository currently contains a total of six items:
- ***Code***: This folder contains two Jupyter notebook files. "20440_project_data_cleanup.ipynb" contains the code to generate a cleaned CSV file of microbial relative abundance data that is saved in the *Data* folder; the contents of this file are explained in the Data section. "UMAP_code_apl.ipynb" contains code for generating the two UMAP figures of diagnosis clustering that are found in the *Figures* folder.
- ***Data***: This folder comprises five subfolders (ASD, AD, PD, MS, epilepsy) corresponding to each of the disorders explored in this work. In each subfolder, there are .txt files containing relative microbial genera abundance data from individuals. The *Data* folder also contains a file called "20440_cleaned_data.csv", which is a tidied concatenation of all the relevant information in the .txt files.
- ***Figures***: This folder contains two SVG images of UMAP plots generated for the microbial relative abundance data for each disorder. "umap_diagnosis.svg" is an image of the UMAP using all the data, which includes "Unknown" and "Unclassified" categories. On the other hand, the "umap_diagnosis_wo_unknowns.svg" plots the UMAP from data excluding these unknown and unclassified categories; the identification and removal of these entries is explained in "Code/UMAP_code_apl.ipynb".
- **.gitignore, .gitattributes**: Files used to configure how `git` performs. The ignore file contains file identities that `git` should ignore when committing and pushing. The attributes file allows for the specification of file and path attributes for `git` to use when performing `git` actions.

### Installation.
All of the code is written in Python in Jupyter, so the user must install Python to run the code, as well as any suitable interface equipped to work with Jupyter notebooks. The Python version used to run this code is 3.11.7. While some of the packages used in the code are built in, there are a few packages that need to be installed. These packages and their versions are listed below:
- jupyterlab: 4.0.11
- pandas: 2.1.4
- numpy: 1.26.4
- umap: 0.5.6
- sklearn: 1.2.2
- seaborn: 0.12.2