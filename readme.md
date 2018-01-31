# CEPure project statements

This project intends to test the CEPure sentence ranking models for automatic summarization tasks.

## Data 

The data set for running this project is located at <http://10.65.0.222/data/cmy_program_dataset/CEsummary_CEPure.rar>

please download it and unzip this file to make sure all subdirectory is located at the home directory of this project.

## Run

The starting code is **cmyDoPyrougeScoreTest.py**

You can use rankthem = 0/1, rougethem = 0/1 to decide whether to rank sentences for each paper in corpus, or do ROUGE evaluation for each model.

The major running function is:

```python
def RunConfig(conf_name, sys_mid_lst rankthem, rougethem, maxw, topk,strictmax, useabstr, writexls, drawfig):
```

Where:

1.  **conf_name**: means the name to build config dict for sorting the configures of ranking and rouge evaluation (see cmyRougeConfig.py), such as "kgmance_exc_withstop_maxword". The first part "kgmance" is the name of corpus (the root directory name of dataset), the second part is whether we include Abstract or Conclusion sentences into ranking process, the third part is whether we remove stopwords from sentences, and the last one is the controller for the length of automatically generated summaries.
2. **sys_mid_list**: is the ID list of models that rank sentences and extract top ranked sentences as summaries. The corresponding between model name and model ID, please refer to  cmyRougeConfig.py.
3. **maxw, topk, strictmax:** these parameters control the length of automatically generated summaries.
4. **useabster**: will affect the value of maxwords. (see OutputTopKSent() in cmyDoRankSent.py)
5. **writexls**:  =1 means we require the ROUGE value be written in a excel table.
6. **drawfig**:  =1 means we will draw bar graphs for each model's ROUGE score.

## Configure Dictionary

For each ranking or rouge evaluation tasks, we will build a dictionary object to store all the configures. It is built from configure name and system model ID list introduced above.

```python
# conf_dict: means the dict object storing the config informations. Contains:
#               u'NameID': The total ranking models' name_to_ID dict
#               u'config_name': the prefix of name for ROUGE evaluation files.
#               u'dataroot': the corpus' root directory
#               u'outdir': the ROUGE evaluation files' direcotry
#               u"pickle_path": the directory which stores the sxpText object pickle files
#               u"celstpk_path": the directory for 'CELink' objects (see cmyPackage.py) list for each paper.
#               u"model_path": the direcotry which stores the model summaries
#               u"system_path": the direcotry which stores the system summaries
#               u'useabstr': whether we use abstract and conclusion in ranking process
#               u'maxword': the max words of the automatically generated summaries
#               u'strictmax': whether we want the length of automatically generated summaries
#                             strictly obey to 'maxword'
#               u'topksent': the number of sentences we extracted to generate summarise
#                            according to their ranking scores
#               u'remove_stopwords': whether we consider stopwords in ranking process
#               u'plotwho':
#               u'system_modelid_list': The list of model IDs that we want to evaluate
#               u'modelpattern': the regular expression pattern of file name of standard summarizes
#               u'systempattern': the regular expression pattern of file name of automatically
#                                 generated summarizes, it will extract model ID.
#               u'model_filenames_pattern_id': the regular expression pattern of file name of
#                                              standard summarizes, it will extract model IDs.
#               u'system_filename_pattern_id': the regular expression pattern of file name
#                                              of automatically generated summarizes.
#               u'pickle_file_pattern_id': the regular expression pattern of file name of
#                                           sxpText pickle file
```

The corresponding dictionary for mapping model name to model ID is:

```python
NameID = {#---------------------- Original models -------------------------
          'keyword':'00','Para':'01', 'TFIDF':'02', 'SimGraph':'03',
          'WordGraph':'04','ParaCtx':'05', 'SecPara':'06',
          'SecParaCtx':'07','Hybrid':'08', 'SecTitle':'09',
          #-------------------- CE Pure models -------------------------
          'MCESimGraph':'10', 'SCESimGraph':'11', 'MCELocPosSeq':'12', 'SCELocPosSeq':'13',
          'MCELocNegSeq':'14', 'SCELocNegSeq':'15','MCESecLocPosSeq':'16', 'SCESecLocPosSeq':'17',
          'MCESecLocNegSeq':'18', 'SCESecLocNegSeq':'19', 'MCETFIDF':'20', 'SCETFIDF':'21'
		 }
```

+ The model name started with "MCE" means we use manually labeled cause-effect links for ranking and evaluating. 
+ The Model name started with "SCE" means we use automatically extracted cause-effect links for ranking and evaluating.

## Introduction for sentence ranking and summarization models

Please refer to ""纯因果关系模型做科技文本自动摘要.docx"