# Les caractéristiques moléculaires latentes issues du séquençage de l'ARN (RNA-seq) permettent-elles de saisir des informations pronostiques associées à la survie globale dans le cancer du sein  sur le dataset TCGA-BRCA

---------------------------------------------------------------------------------------------------------------------------------------------------------

## 1) Contexte du projet
Ce projet interroge les représentations latentes issues de données RNA-seq bulk (TCGA-BRCA) afin de (i) capturer des structures biologiques globales via un autoencodeur (AE), et (ii) être exploitées pour la prédiction de survie à l’aide d’un MLP optimisant une Cox loss 

---------------------------------------------------------------------------------------------------------------------------------------------------------

## 2) Scripts: ￼
	1.	1-QC_NN.ipynb — QC, filtrage gènes/échantillons, split train/test, standardisation, PCA, exports.
	2.	2- AE_NN.ipynb — entraînement AutoEncoder et extraction de l’espace latent.
	3.	3-MLP baseline.ipynb — MLP-Cox sur X_train (baseline).
	4.	4-MLP AE.ipynb — MLP-Cox sur Z_train (latent AE).
	5.	5-MLP_baseline_hyperparam.ipynb — variantes “LR sweep” (baseline).
	6.	6-MLP_AE_hyerparam.ipynb — variantes “LR sweep” (AE).
	7.	7-MLP_baseline_hyperparam_batch.ipynb — variantes “LR × batch” (baseline).
	8.	8-MLP_AE_hyerparam_batch.ipynb — variantes “LR × batch” (AE).
	9.	9-MLP_baseline_hyperparam_lr_on_plateau.ipynb — variantes “Reduce LR on plateau” (baseline).
	10.	10-MLP_AE_hyerparam_lr_on_plateau.ipynb — variantes “Reduce LR on plateau” (AE).

---

## 3) pré-requis
  - Python ≥ 3.10
  - numpy, pandas, scipy
	- scikit-learn
	- matplotlib
	- tensorflow + keras
	- statsmodels
	- gseapy (pour GO/Enrichr)

---

## 4) Installation via conda
	conda create -n m2_ai_dl python=3.11 -y
	conda activate m2_ai_dl
	pip install numpy pandas scipy scikit-learn matplotlib statsmodels
	pip install tensorflow keras
	pip install gseapy
	pip install jupyter ipykernel
	python -m ipykernel install --user --name m2_ai_dl --display-name "m2_ai_dl"

---

## 5) Installation via pip
	python -m venv .venv
	source .venv/bin/activate   # mac/linux
	(.venv\Scripts\activate    # windows)
	
	pip install numpy pandas scipy scikit-learn matplotlib statsmodels
	pip install tensorflow keras
	pip install gseapy
	pip install jupyter ipykernel

---

## 6) Données attendues
Le dépôt ne fournit pas les données TCGA et doivent être récupérées séparément.
Le pipeline Python suppose ensuite la présence (ou la création) d’un dossier de sortie QC :
	- ./tcga_brca_qc_report/ (créé par 1-QC_NN.ipynb)  ￼
Ainsi que:
	-	qc_train.csv, qc_test.csv
	-	￼_derived.csv`
	-	meta_train_postQC.csv, meta_test_postQC.csv
	-	X_train_logCPM_selectedGenes.csv, X_test_logCPM_selectedGenes.csv
	-	X_train_z.npy, X_test_z.npy
	-	gene_names.npy
	-	go_hvg_enrichment.csv (GO sur gènes retenus après QC, selon l’implémentation) 

---

## 7) Execution de la pipeline
### Etape 1: QC et preprocessing
	Ouvrir 1-QC_NN.ipynb et exécuter toutes les cellules.
	Sortie principale : tcga_brca_qc_report/ avec matrices (train/test), métadonnées et gènes

### Etape 2: Autoencoder
	Ouvrir 2- AE_NN.ipynb.
	Objectif : entraîner l’AE et sauvegarder / récupérer les représentations latentes Z_* (train/val/test selon ton split interne)

### Etape 3: Modèle de survie
	Un MLP sur les gène filtrés sans machine learning issus du QC = X_train (./tcgbrca_qc_report/)
	Un MLP sur l'espace latent de l'autoencoder = Z_train
	Les notebooks entraînent un MLP qui produit un score de risque par patient et exportent des fichiers de risques pour une Gene Ontology analysis après ou tout autre analyse.

### Etape 4: Variante d'optimisation avec les hyperparamètres
	-	LR sweep : 5 (baseline) et 6 (AE)
	-	LR × batch : 7 (baseline) et 8 (AE)
	-	LR on plateau : 9 (baseline) et 10 (AE)

---

## 8) Sortie principale attendues
Des performances de survie:
	Courbes d'entraînement Cox-loss et C-index sur validation set.
	Score des C-index sur le dataset test.
Des scores de risques:
	*_risk_test.csv : risques sur l’ensemble test
Interprétation biologique: GO Enrichment
	Pour chaque modèle, une liste de gènes est soumise à Enrichr via gseapy, et les résultats GO sont exportés/visualisés (barplots -log10 p-ajustée).

---
  
