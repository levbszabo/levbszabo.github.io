## Implementation and Evaluation of the DANCER Framework for Academic Article Summarization

Joint Work: Aathira Manoj, Minji Kim

[Paper](/pdf/CovidPaper.pdf)
<br>
[Code](https://github.com/ls5122/COVID)

**Overview** The new daily counts of COVID-19 across the most populous 1250 U.S. counties is predicted using a modified compartmental epidemiological model, denoted NNMRA-SEIR. Modifications to traditional compartmental models include localized measures of nearby outbreak severity through a nearest neighbor heuristic. The ability of NNMRA-SEIR to predict COVID-19 cases is shown to outperform a baseline SEIR model on a multi week prediction task. Further, NNMRA-SEIR indicates a tighter fit around the true infection curve compared to a baseline SEIR model.

### 1. Compartmental Epidemiological Modelling (SEIR)

Compartmental epidemiological models are an approach to modelling the spread of infectious diseases by separating the population into labeled compartments. In the case of an SEIR model these compartments are respectively Susceptible, Exposed, Infected and Recovered/Removed <sup>[1](#seir)</sup>. The SEIR model is just one of the various compartmental models and it builds on the simpler SIR model which makes the simplification that susceptible individuals go directly to becoming infected upon exposure to the agent. 

<img src="images/SEIR.JPG?raw=true"/>

The diagram above indicates the compartments of the SEIR model, every arrow corresponds to a transition between the states governed by the following equations


<img src="images/SEIR_equations.JPG?raw=true"/>

### 2. Problem Statement

Our goal is to evaluate and compare the performance of DANCER by using different scoring metrics for splitting the document summary into different sections. In particular, we use ROUGE-L <sup>[2](#rouge)</sup> and BLEU <sup>[3](#bleu)</sup> scores to generate the section-wise summaries from the main summary for training. We use the Pointer Generator, based on the sequence-to-sequence RNN paradigm, as the main summarization model. Finally, we compare the performance of the DANCER model with a baseline model, which also uses Pointer Generator, but does not split the main summary into section-wise summaries. We use a subset of a publicly available pre-processed ArXiv dataset for our experiments. The performance of different models is evaluated using ROUGE scores. 

A general outline of our approach is given below. 

* Use the DANCER method to generate training datasets using the ROUGE-LCS score and the BLEU-Corpus score. Generate a baseline dataset which does not use this approach.
* Train Pointer Generator Seq2Seq summarizer separately on all these datasets. 
* While testing, use the trained weights of the Pointer Generator model to generate section-wise summaries and concatenate them to generate the final summary .
* Evaluate the different models according to different ROUGE scores.

### 3. Pointer Generator Model

The Pointer Generator <sup>[4](#pointer)</sup> is a sequence to sequence neural model that provides abstract text summarization. The model consists of an encoder and decoder phase. Through a combination of pointing at words in the source text and generating words from the vocabulary distribution this model can be seen as a hybrid summarization model. The model uses a coverage mechanism to minimize the repetition of copied words. 

The input source text is embedded and fed into a bidirectional LSTM, the red network in the diagram. This network serves as the encoder and produces a set of hidden states. Decoding takes place one token at a time. For each timestep the decoder gives rise to decoder states which during training correspond to the word embedding of the previous word. Together these combine to create the attention distribution.

Next using the attention distribution the context vector is constructed as a dot product between the attention distribution and the hidden states. The attention distribution guides the decoder towards the next word while the context vector serves to produce the distribution over all words in the vocabulary P(x) as well as the pointer generator probability W which gives us the option to copy words from the source text according to  S(x), the source distribution. Output tokens are produced according to

``F(x) = W*P(x) + (1-W)*S(x)`` 


<img src="images/white_ptr_gen.png?raw=true"/>


### 4. Results

To test the efficacy of the DANCER framework we compared a baseline Pointer Generator model against one which used the ROUGE-LCS and one which used the BLEU scores to split summaries. We trained on 8000 articles and tested on 1000 articles. The results indicate the ROUGE-LCS as being superior on a variety of metrics when compared to both the BLEU and Baseline model.

<img src="images/summary_comparison2.jpg?raw=true"/>


Finally we show an example of a target abstract with both the ROUGE and BLEU summaries. One can see evidence of both source and vocab distributions.

<img src="images/summary_example.png?raw=true"/>

<a name="seir">[1]</a>: Vynnycky, E.; White, R. G., eds. (2010). An Introduction to Infectious Disease Modelling. Oxford: Oxford University Press.

<a name="rouge">[2]</a>: Chin-Yew  Lin.  2004.   ROUGE:  A  package  for  automatic evaluation of summaries.  In Text Summarization Branches Out, pages 74–81, Barcelona, Spain. Association for Computational Linguistics.

<a name="bleu">[3]</a>: Kishore Papineni, Salim Roukos, Todd Ward, and Wei-Jing  Zhu.  2002. Bleu:  A  method  for  automatic evaluation of machine translation.  ACL ’02,  page 311–318, USA. Association for Computational Linguistics

<a name="pointer">[4]</a>: Abigail See, Peter J. Liu, and Christopher D. Manning. 2017. Get to the point: Summarization with pointer-generator networks.
