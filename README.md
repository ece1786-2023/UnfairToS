# UnfairToS

This is the source code of Unfair ToS project by Jianing Zhang and Jiazhou Liang in ECE1876, Fall 2023. Please used following guides to access the training dataset, models, evaluation metrics to re-create the project.

## Dataset Description

- folder **Dataset** contain the 100 ToS dataset from paper 'Detecting and explaining unfairness in consumer
contracts through memory networks' used for text classification model
    - **ToS-100.csv**: the orignal dataset from the paper, more detail can be found in https://github.com/federicoruggeri/Memnet_ToS
    - **ToS-100-cleaned.csv**: the cleaned dataset with for this project, included the orignal sentences, binary variables for each of the five classes, and a combined variable 'label' for all classes.
    - ToS-100-cleaned-dataset-huggingface: huggingface dataset after oversampling, used for fine-tuning pretrained gpt-2 model. Splitted into 80\% training and 20% testing. More information about how to load the dataset into huggingface library can be found here: https://huggingface.co/docs/datasets/loading

- folder **Dataset for Text Summary Model** contain the ToS document crawled from ToS;DR website with the hightlighted sentences by human contributors for text hightlighting model. More information about how ToS;DR can be found here: https://tosdr.org/
    - **html_files** folder contain the raw crawled ToS in html format
    - **text_files** folder contain the raw crawled ToS in txt format
    - **cleaned** contained the dataset and model results after cleaning and feed into the gpt_4 model
        - **more_than_40_sentences** only included ToS with more than 40 and less than 50 hightlighted sentences from ToS;DR 
        (different range of ToS can be created using script in text_summary.ipynb by changing the parameters indicating in the file. It will create a new folder in the same level as this folder with name " more_than_[lower_constraint]_sentences")
            - **original** contains all ToS text after cleaning, divded in to sentence level and stored in csv format
            - **highlight** cotnains all ToS hightlighted sentence after cleaning
            - **Combined** added a label for each sentences in each ToS in **original** (1) if the sentence is hightlighted, (0) if not. This dataset will be used as ground truth in our eveluation scripts.
            - **Model Result** contain the gpt-4 output using current ToS. More detailed will be explained in result section.
- **data_extraction_all.py** and **data_extraction_highlight.py**  contains the script for crawling raw html and text format of ToS and hightlighted sentences for ToS.
## Model Training and Eveluation
- **Classification_baseline CNN.ipynb** contained the basline CNN model for text classification, more detail about the training process can be found in the files. We used the 100 ToS.csv metioned above.
- **Classification.ipynb** contained the fine-tuning process of pretained gpt-2 model using HuggingFace pretained and auto-tuned interface. More detail about its usage can be found here: https://huggingface.co/docs/transformers/training
- **Text Summary.ipynb** contained the script to cleaned the crawled ToS into csv format at the sentence level, the script using OpenAI API to feed the cleaned ToS into 'gpt-4-turbo-preview' model and created the output. You need to create your own api keys and stored in local enviorment excuete the script. More information about OpenAI api and keys can be found here: https://openai.com/blog/openai-api

## Model Result
- **Text Classification**: The fined-tuned gpt-2 model can be found in following google drive link, due to the size limitation in github. You can used huggingface classification pipline to load the model and make prediction on new sentence, detailed can be found in the last section of **Classification.ipynb** documents.

- **Text Highlighting and Simiplification**: model results and gpt-4 output can be found in 'Dataset for Text Summary Model/cleaned/more_than_40_sentences/model_results', within it, the files with 
    - '_baseline.csv' is the hightlighted sentences for result from baseline textrank model.
    - '_summary.csv' contain the hightlighted sentences and its simiplification for gpt-4 model. To reduce output token sizes, we use index to label each sentences, this required matched it back to original sentences. More detailed can be found in **Classification.ipynb**. 
    -   '_not_predicted.csv' contains all sentences in gpt-4 that is not matched with the ground truth. Please refer to project report to know why these sentences can be important for users.




        