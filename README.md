# Insilico Drug Discovery using Deep Learning
**A simple approach on generating drug sequences using LSTMs** 

**JULY 2020**

In this project we use LSTM's to generate Drug Sequences.

The Model generates SMILES seqeuences of drug sequqences like how a language model trained on shakespeare's poems generates shakespearean poems.

The Preprocessing and training has been documented in the notebook. The notebook will be uploaded on GitHub hopefully.

The idea used in this project is highly infuenced by the paper : https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5836943/

Please go through the notebook it has explaination and reason behind every code.  

This project is open for constructive criticism. Feel free to drop your opinions and changes you feel would improve its perfomance.

**UPDATE : AUGUST 2020**

There have been considerable new changes made in the project these are as follows:

1. SMILES GENERATOR : Constructed a New Model using GRUs with an embedding layer to overcome shortcomings of LSTM Model.

2. Used Reinforcement techniques to generate novel drug sequences (SMILES format) using Molecular finerprints
This approach uses two models(GRUs + Embeding from Point 1). They are called Prior and Agent, Prior and Agent uses the weights we got after training the SMILES Generator(Point 1) and using Experience Relay we and augmented loss likelihood to finuetune the Agent to generate sequnces( read as Sample) with respect to a Molecule (read as Client).

We perfom a scoring by comparing client and sample, the way we do this is by using FingerPrint generator(1024 Dimension Vector) and then we calculate the Tanimoto Similarity between the two and finally we get a score (0-1) which we incorporate into our loss function to finetune the Agent.

Finally we generate sequences with a certain similarity (a hyperparameter we can tweak) w.r.t client's molecule.

**UPDATE : SEPTEMBER 2020**

1.New architectures like Transformers were explored to improve the model, however owing to the limitations of compuataional power. I could not continue my research in that direction.

2. To improve the perfomance I started working on the fingerprint generator.Graph Neural Networks were a better choice due to the homologous similairty between molecular structure and architecure structure. (Many differnt models were trued and DMPNNS were experimented and the research is still ongoing).

![Working of GNNs Picture taken from DMPNN Paper](https://pubs.acs.org/na101/home/literatum/publisher/achs/journals/content/jcisd8/2019/jcisd8.2019.59.issue-8/acs.jcim.9b00237/20190819/images/medium/ci9b00237_0001.gif)

*Working of GNNs in DMPNNs Picture taken from DMPNN Paper*

Similarly, Graph Transformers were also ideal candidate for the job. Molecule Attention Transformer MAT was experimented and the arcitecture was changed to get a 1024 Dimension feature vector. Thankfully the authors have provided pretrained weights which were used to generate 1024 Dimesional feature vector. The research is still ongoing (currently paused due to other commitments).

![Architecture Of MAT](https://github.com/gmum/MAT/raw/master/assets/MAT.png)

*Architecture Of MAT*

Once this architecture is incorporated into the model, I am planning to constrcut a web API (similar to http://chemprop.csail.mit.edu/predict) to genearete suitable drug candidates with respect a client molecule with a certain similairty(hyperparameter).

This project is open to further research. Feel free to drop your opinions and changes you feel would improve its perfomance.







