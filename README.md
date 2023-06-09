
## MSc Thesis - Naomi Rood
### Information Studies, Track Data Science

Welcome to the GitHub page of Link Prediction for the Context of Dutch News Articles using the Graph Neural Network SEAL Framework with CANE Embeddings. 
<br />

## In short
With the proliferation of news articles, effectively managing information overload by providing relevant recommendations is critical. Graph Neural Networks have emerged as a promising approach for news recommendation systems through link prediction. The SEAL framework, a novel approach in link prediction research, utilizes enclosed subgraphs, node embeddings, and node attributes as inputs to train a Graph Neural Network that outputs a probability of link existence. However, it does not incorporate text information in its node embeddings. This study proposed a method for link prediction that combines the state-of-the-art framework SEAL with Context-Aware Network Embeddings (CANE). CANE integrates information from the text of surrounding nodes and the graph structure into a node embedding. Additionally, a new dataset from the NOS with 468k Dutch news articles and hand-labeled links representing article recommendations is presented. Evaluation on the NOS dataset using article text showed that SEAL with CANE embeddings (AUC = 88.53) has an improved ability to discriminate positive and negative links compared to SEAL (subgraphs only) (AUC = 80.69) and SEAL with Node2vec embeddings (AUC = 82.21). However, it is noteworthy that the text-only method TF-IDF outperforms the proposed method in recommending articles.  

## Project Folder Structure

- [``Code``](Code/): The Python code, the data folder, and the EDA notebook. 
- [``Thesis``](Thesis/): The final thesis.

## Requirements and Installation
To clone this repository:
```
git clone https://github.com/Naomirood/Thesis
```
Use the following line to install the required software and libraries, run the line in the [``Code``](Code/) folder. It will download and install the default Graph Neural Network software of Zhang and Chen (2018) [pytorch_DGCNN](https://github.com/muhanzhang/pytorch_DGCNN) to the same level as the root folder (only suitable for MacOS and Linux).
```
bash ./install.sh
```
The code is last tested on the versions of the following dependencies:
<br />
Python == 3.7.9 <br />
TensorFlow == 2.12.2 <br />

## Usage
To train the model and predict scores, run one of the following lines in the [``Python``](Code/Python) folder. <br />

To run the code, it is necessary to have a .mat file in the [``data``](Code/Python/data) folder. It must contain a *net* variable with all the links in the original network, it is optional to have a *group* variable with the attributes of the nodes. If the nodes have corresponding text, these must be in the *text* variable. Example data of NOS articles and links of one month is provided in the  [``data``](Code/Python/data) folder. <br />

To run the model and have a try of the framework, type:
```
python3 Main.py --data-name 1m_NOS --use-cane-embedding --hop 'auto' 
```
this will automatically output the AUC score and uses the default test split of 10% (append "--test-ratio" to change this ratio). To use the SEAL method with only subgraphs, don't use embeddings. Instead of using CANE embedding, it is also possible to use Node2vec embeddings (for example when having no *text* in the .mat file). Use then: --use-n2v-embedding. Hop number set automatically from {1,2}, if you want to select a specific hop number, set "--hop 1". <br />

To try SEAL + CANE on a custom split of the links, it is necessary to make .txt files of the train, test, and validation set and have them in the [``data``](Code/Python/data) folder. <br />
If you want to output the link existence probabilities on the test set, first type:
```
 python3 Main.py --data-name 1m_NOS --use-cane-embedding --hop 'auto' --train-name links_1m_train.txt --test-name links_1m_val.txt --save-model
```
here, the "--save-model" saves the final model to "data/data_name.pth". To predict link existence probabilities to the test set (may contain both positive and negative links), type to load the saved model and output predictions to "data/test_name_pred.txt":
```
 python3 Main.py --data-name 1m_NOS --use-cane-embedding --hop 'auto' --train-name links_1m_train.txt --test-name links_1m_val.txt --only-predict
```
The "--only-predict" argument will automatically print the recall scores (thresholds of 0.5, 0.7, and 0.9) for the test set. If you want that the model learns from node attributes as well, append "--use-attribute". This is only possible when a *group* is within the .mat file.

To change the number of epochs, layers of the GNN or a different parameter for the SEAL training process, you can update the parameters in [``Main.py``](Code/Python/Main.py). <br />
To change the number of epochs, max_len of the text or a different parameter for the CANE training process, you can update [``config.py``](Code/Python/config.py). <br />

<br /> 
For a complete overview of the arguments, look at the Main.py file. <br /> 

## Acknowledgements

This research was conducted as a master thesis Information Studies: data science at the University of Amsterdam. This work is written in collaboration with the NOS (the Dutch Public Broadcasting Foundation). 
<br />
Code has been used from the [CANE](https://github.com/thunlp/CANE/tree/master/code) and [SEAL](https://github.com/muhanzhang/SEAL/tree/master/Python) GitHub repository. The PyTorch implementation of DGCNN is also used from the [PyTorch DGCNN](https://github.com/muhanzhang/pytorch_DGCNN) GitHub repository. 
<br />

References of the papers of those repositories: <br />
Cunchao Tu, Han Liu, Zhiyuan Liu, Maosong Sun. *CANE: Context-Aware Network Embedding for Relation Modeling.* The 55th Annual Meeting of the Association for Computational Linguistics (ACL 2017).
<br />
Zhang, M., & Chen, Y. (2018). *Link prediction based on graph neural networks. Advances in neural information processing systems*, 31.
<br />
Zhang, M., Cui, Z., Neumann, M., & Chen, Y. (2018, April). *An end-to-end deep learning architecture for graph classification*. In Proceedings of the AAAI conference on artificial intelligence (Vol. 32, No. 1).
