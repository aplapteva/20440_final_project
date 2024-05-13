# Comorbidity and exclusion of neurological disorders predicted by human gastrointestinal microbiome compositions.
## Allison Glynn and Anna Lapteva  $\text{ }\mid\mid\text{ }$   May 13, 2024

### Overview.
This repository contains the code, data, and figures for the 20.440 project titled "Comorbidity and exclusion of neurological disorders predicted by human gastrointestinal microbiome compositions". The goal of this project is to identify shared and distinct features of human gut microbiome composition across four neurological disorders: Austism Spectrum Disorder (ASD), Alzheimer's Disease (AD), Parkinson's Disease (PD), and Multiple Sclerosis (MS). This repository includes the necessary materials for creating and tidying a pooled dataset of microbial relative abundance data, performing clustering on pooled and paired datasets, and building a classifier that predicts disease phenotype based on microbial relative abundance profile.

### Data.
The data used in this project comes from the [Data Repository for Human Gut Microbiota (GMRepo)](https://gmrepo.humangut.info/home), which facilitates reliable comparison of datasets sourced from different studies. The paper describing the creation of this database can be found [here](https://academic.oup.com/nar/article/50/D1/D777/6426060?login=true). As the database does not currently allow for the download of all data simultaneously for a single phenotype within a project, the following workflow was used to gather .txt files containing relative abundance data of microbial genera. 

For each phenotype:
1. Look up phenotype of interest in the search bar at the top of the [GMRepo home page](https://gmrepo.humangut.info/home).
2. Navigate to "Related projects" and click on the ProjectID that satisfies the three criteria detailed in the report. 
3. Open the ProjectID and click "Download all runs as tsv."
4. Open the TSV file and add a column containing the link to each run. This can be achieved by the formula "=CONCAT("https://gmrepo.humangut.info/data/run/", run ID cell number)". The resulting link for run ID ERR1072832 should thus be https://gmrepo.humangut.info/data/run/ERR1072832.
5. Navigate to the QC Status column in the TSV file -- it should be the last column. Filter it to only show entries with QC Status = 1.
6. Copy all links for QC Status = 1 and open them as individual tabs in a new window. This can likely be done in many ways, but I used the [Copy All URLs](https://chromewebstore.google.com/detail/copy-all-urls/djdmadneanknadilpjiknlnanaolmbfk) extension on Google Chrome to open all copied links simultaneously.
7. For each tab, which corresponds to a single run (i.e. one individual's relative abundance data), click "Download relative genus abundance data as tsv". Close the tab and move onto the next tab.
8. Move all downloaded files into the appropriate folder (e.g. all ASD files are moved into ASD folder).

The above process was repeated for all four conditions. 

### Folder Structure and Files.
This repository currently contains a total of six items:
- ***Code***: This folder contains Jupyter notebook files with code for performing clustering and classification
    - "20440_project_data_cleanup.ipynb" contains the code to generate a cleaned CSV file of microbial relative abundance data pooled for all four disorders.
    - "UMAP_code_apl.ipynb" contains code for generating the UMAP clustering.
    - "paired_analysis_ag.ipynb"
    - "pooled_analysis_ag.ipynb"
- ***Data***: This folder comprises four subfolders for each disease, a feature importance folder, and a cleaned dataset. 
    - "ASD" contains .txt files with relative microbial genera abundance data from individuals with ASD
    - "AD" contains .txt files with relative microbial genera abundance data from individuals with AD
    - "PD" contains .txt files with relative microbial genera abundance data from individuals with PD
    - "MS" contains .txt files with relative microbial genera abundance data from individuals with MS
    - "feature_importance"
    - "20440_cleaned_data.csv" is a tidied concatenation of all relevant information in all .txt files
- ***Figures***: This folder contains SVG images of plots generated for the report.
- **.gitignore, .gitattributes**: Files used to configure how `git` performs. The ignore file contains file identities that `git` should ignore when committing and pushing. The attributes file allows for the specification of file and path attributes for `git` to use when performing `git` actions.

### Installation.
All of the code is written in Python in Jupyter, so the user must install Python to run the code, as well as any suitable interface equipped to work with Jupyter notebooks. The Python version used to run this code is 3.11.7. While some of the packages used in the code are built in, there are a few packages that need to be installed. These packages and their versions are listed below:
- jupyterlab: 4.0.11
- pandas: 2.1.4
- numpy: 1.26.4
- umap: 0.5.6
- sklearn: 1.2.2
- seaborn: 0.12.2
- 