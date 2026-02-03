# Les caractéristiques moléculaires latentes issues du séquençage de l'ARN (RNA-seq) permettent-elles de saisir des informations pronostiques associées à la survie globale dans le cancer du sein  sur le dataset TCGA-BRCA



## 2- Scripts: ￼
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

  ## 3 pré-requis
  - Python ≥ 3.10
  - numpy, pandas, scipy
	- scikit-learn
	- matplotlib
	- tensorflow + keras
	- statsmodels
	- gseapy (pour GO/Enrichr)

 ## 4 Installation via conda
  conda create -n m2_ai_dl python=3.11 -y
  conda activate m2_ai_dl
  pip install numpy pandas scipy scikit-learn matplotlib statsmodels
  pip install tensorflow keras
  pip install gseapy
  pip install jupyter ipykernel
  python -m ipykernel install --user --name m2_ai_dl --display-name "m2_ai_dl"

## 5 Installation via pip
python -m venv .venv
source .venv/bin/activate   # mac/linux
(.venv\Scripts\activate    # windows)

pip install numpy pandas scipy scikit-learn matplotlib statsmodels
pip install tensorflow keras
pip install gseapy
pip install jupyter ipykernel






  
