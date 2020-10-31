# Insilico Drug Discovery using Deep Learning




**JULY 2020**

**A simple approach on generating drug sequences using RNNs & Transformers** 

* In this project we use LSTM's to generate Drug Sequences.

* The Model generates SMILES seqeuences of drug sequqences like how a language model trained on shakespeare's poems generates shakespearean poems.

* The Preprocessing and training has been documented in the notebook. The notebook will be uploaded on GitHub hopefully.

* The idea used in this project is highly infuenced by the paper : https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5836943/

* Please go through the notebook(de-novo-drug-synthesis-using-rnns.ipynb) it has explaination and reason behind every code.  

Feel free to drop your opinions and changes you feel would improve its perfomance.




**UPDATE : AUGUST 2020**

* There have been considerable new changes made in the project these are as follows:

   1. SMILES GENERATOR : Constructed a New Model using GRUs with an embedding layer to overcome shortcomings of LSTM Model.

   2. Used Reinforcement techniques to generate novel drug sequences (SMILES format) using Molecular finerprints
   
* This approach uses two models(GRUs + Embeding from Point 1). They are called Prior and Agent, Prior and Agent uses the weights we got after training the SMILES Generator(Point 1) and using Experience Relay we and augmented loss likelihood to finuetune the Agent to generate sequnces( read as Sample) with respect to a Molecule (read as Client).

* We perfom a scoring by comparing client and sample, the way we do this is by using FingerPrint generator(1024 Dimension Vector) and then we calculate the Tanimoto Similarity (Jaccard Similarity) between the two and finally we get a score (0-1) which we incorporate into our loss function to finetune the Agent.

Finally we generate sequences(read as candidates) with a certain similarity (a hyperparameter we can tweak) w.r.t client molecule.



**UPDATE : SEPTEMBER 2020**

   * New architectures like Transformers were explored to improve the model, however owing to the limitations of compuataional power. I could not continue my research in that direction.

   *  To improve the perfomance I started working on the fingerprint generator. Graph Neural Networks were a better choice due to the homologous similairty between molecular structure and architecure structure.(Many differnt models were trued and [DMPNNS](https://pubs.acs.org/doi/full/10.1021/acs.jcim.9b00237) were experimented and the research is still ongoing).

![Working of GNNs Picture taken from DMPNN Paper](https://pubs.acs.org/na101/home/literatum/publisher/achs/journals/content/jcisd8/2019/jcisd8.2019.59.issue-8/acs.jcim.9b00237/20190819/images/medium/ci9b00237_0001.gif)

           Working of GNNs in DMPNNs Picture taken from DMPNN Paper
               
* Since I needed to extract features from the seqeunces and compare it with another molecule, I tried applying Autoencoders, Comparing two latent represenations would be similar to fingerprint similarity between two moeclues, TRansformers were use for this purpose, SMILES Transformers was used and its diiferent layers were merge to get a 1024 Dimensional Vector which would be used for comaprison. The Transfromer(Decoder) would try to recreate the molecule from the latent vector (Output from Encoder). This approach perfromed similar to Domain Specific Molecular Finergerprint and there was no room for improvement until I thought of merging both the architectures(Graphs & Transformers). To get best of both worlds with the lest trade off, Graph Ttransfromers were explored. 

  ![](https://user-images.githubusercontent.com/47039231/95646485-85e40080-0ae6-11eb-88a1-1c162a96d079.png)

                                          SMILES Transformer Working

* Transformers using graphs were also ideal candidate for the job. [Molecule Attention Transformer(MAT)](https://arxiv.org/abs/2002.08264) was experimented and the arcitecture was changed to get a 1024 Dimension feature vector. Thankfully the authors have provided pretrained weights which were used to generate 1024 Dimesional feature vector. The research is still ongoing (currently paused due to other commitments). You cn read the Project Report for more details on the project.

![Architecture Of MAT](https://github.com/gmum/MAT/raw/master/assets/MAT.png)

                                       Architecture Of MAT


* Once this architecture is incorporated into the model, I am planning to construct a web API (similar to http://chemprop.csail.mit.edu/predict) to genearete suitable drug candidates with respect a client molecule with a certain similairty(hyperparameter).


This project is open to further research. Feel free to drop your opinions and changes you feel would improve its perfomance.


**UPDATE**

* The MAT has been incorporated inside the main Model. Currently the Two Stage Model Supports Cosine Similarity for scoring when you use MAT for fingerprint generation. If you wish to use domain specific RDkit Morgan Fingerprint, Tanimoto Similairty is used. Different Similairty measures are being experimented. MAT also will be changed in the future iterations of the project. I have updated main.ipynb notebook which has the code for Complete Two Stage Model. Scripting is underway. After its done .ipynb will be replaced with the final script. Cheers!

* Caveat: 

I have noticed that MAT is underperfroming owing to the lack of training on a relevant dataset corresponding to the client's molecule. Let's say the Client is looking for Cancer related drug sequences. In ordedr to give specific to cancer realted drugs MAT needs to be trained on a certain downstream task like predicting Anti Cancer properites as a regression/classfication problem. Later use trained transformer weights in the Fingerprint Generator to get a 1024 vector (by making few changes of course). MAT initally was made for classification/Regression subproblems. Using it as a Fingerprint Generator by modifiyng few final layers is the magic here! :D 







