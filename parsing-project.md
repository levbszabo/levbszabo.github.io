## Implementation of a Probabilistic CYK Parser

[Code](https://github.com/ls5122/CYKParser)

**Overview** We wish to parse sentences given specific grammar rules and their production probabilities over a corpus. Using the added probabilities of production rules and bottom up dynamic programming, more specifically the CYK Parsing Algorithm we can generate the most likely parse tree. The parsing algorithm is tested on a subset of the WSJ treebank dataset. Over 10,000 grammar production rules are utilized to produce parse trees for new sentences.


### 1. Parsing

Parsing in NLP is the process of determining the syntactic structure of a text by analyzing its words according to an underlying grammar. A grammar is composed of multiple terminals (tokens), non-terminals (grammatical entities such as "noun phrase"), and associated rules that generate the terminals and non-terminals. An example of a grammar and a parse tree for the sentence **"the giraffe dreams"** is shown below <sup>[1](#parse_example)</sup> . Successfully parsing novel sentences is a pivotal component of many NLP applications including machine translation  as well as infering semantic meaning as evidenced in question answering systems. 


<img src="images/ParseTreeExample.JPG?raw=true"/>

### 2. CYK Parsing

The extended Cocke–Younger–Kasami (CYK) algorithm for given probabilistic context-free grammars (PCFGs) is an inference algorithm that utilizes dynamic programming to find the most likely parse tree of a given sentence according to production probabilities. The CYK algorithm has a worst case running time of ``O(n^3 * |G|)`` where G is the size of the grammar. The CYK algorithm requires our Context Free Grammar (CFG) G to be in Chomsky Normal Form meaning that all production rules either produce 1 or 2 nonterminals or exactly 1 terminal. A probablistic context free grammar (PCFG) consists of Σ : terminals, N : non-terminals, R: production rules, S: start symbol.
 
``
**Input:** A string x1...xn and PCFG = (Σ, N, R, S)


initialize bestScore[i,j][A] = 0  for all 0<=i<j<=n , A in N 


initialize backpointer
``

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

<a name="parse_example">[1]</a>: Allen, James, Natural Language Understanding 2e, Benjamin Cummings, 1995. 

<a name="rouge">[2]</a>: Chin-Yew  Lin.  2004.   ROUGE:  A  package  for  automatic evaluation of summaries.  In Text Summarization Branches Out, pages 74–81, Barcelona, Spain. Association for Computational Linguistics.

<a name="bleu">[3]</a>: Kishore Papineni, Salim Roukos, Todd Ward, and Wei-Jing  Zhu.  2002. Bleu:  A  method  for  automatic evaluation of machine translation.  ACL ’02,  page 311–318, USA. Association for Computational Linguistics

<a name="pointer">[4]</a>: Abigail See, Peter J. Liu, and Christopher D. Manning. 2017. Get to the point: Summarization with pointer-generator networks.
