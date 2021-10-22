# Information retrieval:

## Chapters:

1. [_Modeling_](#c1)
   1. [_IR Models_](#c1.1)
   2. [_Classical Similarity Models_](#c1.2)
      1. [_Boolean model_](#c1.2.1)
      2. [_Vector Space model_](#c1.2.2)
      3. [_Probabilistic model_](#c1.2.3)
   3. [_Alternative Probabilistic Models_](#c1.3)
      1. [_BM25_](#c1.3.1)
      2. [_Language Model_](#c1.3.2)
2. [_Retrieval Evaluation_](#c2)
3. [_Relevance Feedback and Query Expansion_](#c3)
4. [_Text Operation_](#c4)
5. [_Indexing and Searching_](#c5)
6. [_Web Retrieval_](#c6)
7. [_Multimedia Information Retrieval_](#c7)
   1. [_Image Retrieval_](#c7.1)
   2. [_Video Retrieval_](#c7.2)
   3. [_Audio Retrieval_](#c7.3)

## Assignments:

- [x] [Øving 1](https://github.com/Hoyby/NTNU/tree/master/TDT4117-InfoRet/Assignment_1)
- [ ] [Øving 2](https://github.com/Hoyby/NTNU/tree/master/TDT4117-InfoRet/Assignment_1)
- [ ] [Øving 3](https://github.com/Hoyby/NTNU/tree/master/TDT4117-InfoRet/Assignment_1)
- [ ] [Øving 4](https://github.com/Hoyby/NTNU/tree/master/TDT4117-InfoRet/Assignment_1)
- [ ] [Øving 5](https://github.com/Hoyby/NTNU/tree/master/TDT4117-InfoRet/Assignment_1)

</br></br></br>

# Modeling <a name="c1"></a>

## IR Models <a name="c1.1"></a>

Modeling in Information Retrieval (IR) is a process aimed at producing a ranking function that assigns scores to documents for a given query.

Objectives:

- Fast (millisecond response time) and fault tolerant.
- Full-text search.
- Ordering of documents according to relevance to the user.
- User-centric presentation of results.

> An IR model can be defined by a quadruple: `<D, Q, F, R(q,d)>` where
>
> - D = Document collection
> - Q = Query collection reflecting users information needs
> - F = Framework for modeling documents d, queries q, and their relationship
> - R(q, d) = Ranking function

> - Collection frequency (cf): times term t occurs in collection
> - Document frequency (df): no. of documents d in which the term t occurs. </br>
>   Naturally: df < cf

</br></br>

## Classical Similarity Models <a name="c1.2"></a>

> **An index term**: is a word or expression, which may be stemmed, describing or characterizing a document.
>
> A varaiety of [text operations](#c4) can be done to reduce the number of terms to consider when performing indexing.

</br></br>

**Term and Document frequency | TF-IDF:**

_[Taken from the wikipendium.](https://www.wikipendium.no/TDT4117_Information_retrieval#term-and-document-frequency-tf-idf)_

The **raw frequency** of a term in a document is simply how many times it occurs, but the relevance of the document does not increase proportionally with the term frequency (10 more occurrences does not mean 10 times more relevant). **Log frequency** weighting lowers this ratio:

![tf_{i,j} = \left{ 
    \begin{array}{rr}
        1+\log f_{i,j} \quad \text{if } f_{i,j} > 0 
        0 \quad \text{otherwise}
    \end{array} \right.](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+tf_%7Bi%2Cj%7D+%3D+%5Cleft%5C%7B+%0A++++%5Cbegin%7Barray%7D%7Brr%7D%0A++++++++1%2B%5Clog+f_%7Bi%2Cj%7D+%5Cquad+%5Ctext%7Bif+%7D+f_%7Bi%2Cj%7D+%3E+0+%5C%5C%0A++++++++0+%5Cquad+%5Ctext%7Botherwise%7D%0A++++%5Cend%7Barray%7D+%5Cright.)

<!-- $$tf_{i,j} = \left\{
    \begin{array}{rr}
        1+\log f_{i,j} \quad \text{if } f_{i,j} > 0 \\
        0 \quad \text{otherwise}
    \end{array} \right.$$ -->

The logarithm base is not important, just be concise with the choice.
We want high scores for frequent terms, but we want even higher score for a rare, descriptive term. These terms introduce a good discrimination value, and their score is captured in the **Inverse Document Frequency (IDF)**:

![idf_i = \log \frac{N}{n_i}](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+idf_i+%3D+%5Clog+%5Cfrac%7BN%7D%7Bn_i%7D)

<!-- $$ idf_i = \log \frac{N}{n_i} $$ -->

Where $N$ is the total number of documents in the collection and $n_i$ is the number of documents which contain keyword $k_i$.

IDF provides a foundation for modern term weighting schemes, and is used for ranking in almost all IR systems. </br>
**There is 5 distinct variants:**

- Unary
- Inverse frequency
- Inverse frequency smooth
- Inverse frequency max
- Probabilistic inverse frequency

</br>

If we combine these two scores we get the best known weighting scheme in IR: **the TF-IDF weighting scheme**. This measure increases with the number of occurrences within a document and with the rarity of the terms in the collection.

![w_{i,j} = \left{
	\begin{array}{r r}
    	(1+\log f_{i,j}) \cdot \log\frac{N}{n_i}\quad \text{if} f_{ij} > 0 
    	0 	\quad \text{otherwise}
  \end{array} \right.](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+w_%7Bi%2Cj%7D+%3D+%5Cleft%5C%7B%0A%09%5Cbegin%7Barray%7D%7Br+r%7D%0A++++%09%281%2B%5Clog+f_%7Bi%2Cj%7D%29+%5Ccdot+%5Clog%5Cfrac%7BN%7D%7Bn_i%7D%5Cquad+%5Ctext%7Bif%7D+f_%7Bij%7D+%3E+0+%5C%5C%0A++++%090+%09%5Cquad+%5Ctext%7Botherwise%7D%0A++%5Cend%7Barray%7D+%5Cright.)

<!-- $$w_{i,j} = \left\{
	\begin{array}{r r}
    	(1+\log f_{i,j}) \cdot \log\frac{N}{n_i} \quad \text{if $f_{i,j} > 0$}\\
    	0 	\quad \text{otherwise}
  \end{array} \right.$$ -->

</br>

### Boolean model <a name="c1.2.1"></a>

</br>

**Basic Assumption of Boolean Model**:

1. An index term is either present(1) or absent(0) in the document
2. All index terms provide equal evidence with respect to information needs.
3. Queries are Boolean combinations of index terms.
   1. X AND Y: represents doc that contains both X and Y
   2. X OR Y: represents doc that contains either X or Y
   3. NOT X: represents the doc that do not contain X

Uses a `term-document` matrix to find matches in the queries.

Important drawback using this (basic boolean) model:

1. There is no ranking.
2. There is no partial matching.
3. The ratio between no. of columns (vocabulary) and actually used words (1’s in the matrix) is very low. This will result in a matrix with a LOT of 0’s, and therefore a lot of memory usage, storing no useful information. In practice we therefore only save the entries having 1’s in a form of adjacency list.

</br>

### Vector Space model <a name="c1.2.2"></a>

</br>

> **_Statistical term specificity:_** the inverse of the number of documents in which the term occurs. </br> > **_Zipf’s law:_** model of distribution of terms in a collection.

The **Ranking** in the Vector Space model is done by modeling the query and documents as vectors. The similarity between a query and a document is calculated with the **Cosine Similarity**:

![sim(d_j , q) = \frac{\vec{d_j} \cdot \vec{q}}{\left|\vec{d_j}\right|\left|\vec{q}\right|} = \frac{\sum _{i=1}^t w_{ij}\cdot w_{iq} }{\sqrt{\sum _{i=1}^tw_{ij}^2}\cdot \sqrt{\sum _{i=1}^tw_{iq}^2}}](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+sim%28d_j+%2C+q%29+%3D+%5Cfrac%7B%5Cvec%7Bd_j%7D+%5Ccdot+%5Cvec%7Bq%7D%7D%7B%5Cleft%7C%5Cvec%7Bd_j%7D%5Cright%7C%5Cleft%7C%5Cvec%7Bq%7D%5Cright%7C%7D+%3D+%5Cfrac%7B%5Csum+_%7Bi%3D1%7D%5Et+w_%7Bij%7D%5Ccdot+w_%7Biq%7D+%7D%7B%5Csqrt%7B%5Csum+_%7Bi%3D1%7D%5Etw_%7Bij%7D%5E2%7D%5Ccdot+%5Csqrt%7B%5Csum+_%7Bi%3D1%7D%5Etw_%7Biq%7D%5E2%7D%7D)

<!-- $$ sim(d_j , q) = \frac{\vec{d_j} \cdot \vec{q}}{\left|\vec{d_j}\right|\left|\vec{q}\right|} = \frac{\sum _{i=1}^t w_{ij}\cdot w_{iq} }{\sqrt{\sum _{i=1}^tw_{ij}^2}\cdot \sqrt{\sum _{i=1}^tw_{iq}^2}} $$ -->

This basically measures how much the query vector and the document vector are "pointing in the same direction".

**Problem:** longer documents are more likely to be retrieved by a given query due to the number of words. </br>
**Solution:** document length normalization.

> Document length normalization adjusts the term frequency or the relevance score in order to normalize the effect of document length on the document ranking.

The **disadvantage** of the model is that it assumes all terms are independent.

</br>

### Probabilistic model <a name="c1.2.3"></a>

_[Taken from the wikipendium.](https://www.wikipendium.no/TDT4117_Information_retrieval#term-and-document-frequency-tf-idf)_

There exists a subset of the documents collection that are relevant to a given query. A probabilistic retrieval model ranks this set by the probability that the document is relevant to the query. The advantage of this model is that documents are ranked in decreasing order of their probability of being relevant. The main disadvantage is the need to guess the initial separation of documents into relevant and non-relevant sets.

**Ranking** in the Probabilistic model: The similarity function uses the **Robertsen-Sparck Jones** equation:

![sim(d_j , q) \sim \sum_{k_i \in q \wedge k_i \in d_j } \log \frac{N + .5}{n_i + .5}](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+sim%28d_j+%2C+q%29+%5Csim+%5Csum_%7Bk_i+%5Cin+q+%5Cwedge+k_i+%5Cin+d_j+%7D+%5Clog+%5Cfrac%7BN+%2B+.5%7D%7Bn_i+%2B+.5%7D)

<!-- $$sim(d_j , q) \sim \sum_{k_i \in q \wedge k_i \in d_j } \log \frac{N + .5}{n_i + .5}$$ -->

</br>

## Alternative Probabilistic Models <a name="c1.3"></a>

_[Taken from the wikipendium.](https://www.wikipendium.no/TDT4117_Information_retrieval#term-and-document-frequency-tf-idf)_
</br>

### BM25 <a name="c1.3.1"></a>

Extends the classic probabilistic model with information on term frequency and document length normalization. The ranking formula is a combination of the equation for BM11 and BM15, and can be written as

![sim_{BM25}(D, Q) = \sum _{i=1}^n IDF(q_i) \cdot \frac{f(q_i, D) \cdot (k_1 + 1)}{f(q_i, D) + k_1 \cdot (1 - b + b \cdot \frac{|D|}{avgdl})} ](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle+sim_%7BBM25%7D%28D%2C+Q%29+%3D+%5Csum+_%7Bi%3D1%7D%5En+IDF%28q_i%29+%5Ccdot+%5Cfrac%7Bf%28q_i%2C+D%29+%5Ccdot+%28k_1+%2B+1%29%7D%7Bf%28q_i%2C+D%29+%2B+k_1+%5Ccdot+%281+-+b+%2B+b+%5Ccdot+%5Cfrac%7B%7CD%7C%7D%7Bavgdl%7D%29%7D+)

<!-- $$ sim_{BM25}(D, Q) = \sum _{i=1}^n IDF(q_i) \cdot \frac{f(q_i, D) \cdot (k_1 + 1)}{f(q_i, D) + k_1 \cdot (1 - b + b \cdot \frac{|D|}{avgdl})} $$ -->

where $f(q_i, D)$ is $q_i$'s term frequency in the document $D$, $|D|$ is the length of the document $D$ in words, $avgdl$ is the average document length in the text collection, $k_1$ and $b$ are free parameters, and $IDF$ is the inverse document frequency given by

![ IDF(q_i) = \log \frac{N - n(q_i) + 0.5}{n(q_i) + 0.5} ](https://render.githubusercontent.com/render/math?math=%5Cdisplaystyle++IDF%28q_i%29+%3D+%5Clog+%5Cfrac%7BN+-+n%28q_i%29+%2B+0.5%7D%7Bn%28q_i%29+%2B+0.5%7D+)

<!-- $$ IDF(q_i) = \log \frac{N - n(q_i) + 0.5}{n(q_i) + 0.5} $$ -->

where $N$ is the total number of documents in the collection, and $n(q_i)$ is the number of documents containing $q_i$.

</br>

### Language Model <a name="c1.3.2"></a>

Language modeling (LM) is the use of various statistical and probabilistic techniques to determine the probability of a given sequence of words occurring in a sentence. The idea is to define a language model for each document in the collection and use it to inquire about the likelihood of generating av given query, instead of using the query to predict the likelihood of observing a document.

Example:

![Image](./img/StatisticalLanguageModel.JPG)

query: frog said that toad likes frog STOP. \
**P**(query|M<sub>d2</sub>) = 0.01 ∙ 0.03 ∙ 0.05 ∙ 0.02 ∙ 0.02 ∙ 0.01 ∙ 0.02 = 12 ∙ 10<sup>-12</sup>\
(**P**(query|M<sub>d1</sub>) = 4.8 ∙ 10<sup>-12</sup>) **<** (**P**(query|M<sub>d2</sub>) = 12 ∙ 10<sup>-12</sup>)\
Thus, document d<sub>2</sub> is more relevant to the query than d<sub>1</sub> is.

In practice, we would use log for theese small numbers to prevent underflow.

A problem appears if the likelihood of a term is 0 (non-occuring), as this will make (**P**(q|M<sub>d</sub>) = 0. \
To solve this we need to smooth the estimates.

</br>

#### **Jelinek-Mercer smoothing:**

𝑝(𝑞|𝑑,𝐶) = 𝜆 ∙ 𝑝(𝑞|𝑀<sub>𝑑</sub>) + (1 − 𝜆) ∙ 𝑝(𝑞|𝑀<sub>c</sub>)

- Mixes the probability from the document with the general collection
  frequency of the word
- High value of λ: “conjunctive-like” search – tends to retrieve documents
  containing all query words.
- Low value of λ: more disjunctive, suitable for long queries
- Tuning λ is very important for good performance.

**Mixture model:**

- The user has some background knowledge about the collection and has a “document in mind” and generates the query from this document.

![image](./img/MixtureModel.JPG)

- The equation represents the probability that the document that the
  user had in mind was in fact this one.

**Example:**\
𝑑<sub>1</sub> = _jackson was one of the most talented entertainers of all time_ \
𝑑<sub>2</sub> = _michael jackson anointed himself king of pop_ \
𝑞 = _michael jackson_ \
𝜆 = 1/2 (mixture model) \
𝑝(𝑞|𝑑<sub>1</sub>) = [(0/11 + 1/18) / 2] ∙ [(1/11 + 2/18) / 2] = 0.003 \
𝑝(𝑞|𝑑<sub>2</sub>) = [(1/7 + 1/18) / 2] ∙ [(1/7 + 2/18) / 2] = 0.013 \
Ranking: 𝑑<sub>2</sub> > 𝑑<sub>1</sub>.

</br>

#### **Dirichlet smoothing:**

The background distribution P(t|M<sub>c</sub>) if the prior for P(t|𝑑).

TODO: Formula

</br>

> For long queries, the Jelinek-Mercer smoothing performs better than
> the Dirichlet smoothing.

> For short queries, the Dirichlet smoothing performs better than the
> Jelinek-Mercer smoothing

</br></br></br>

# Retrieval Evaluation <a name="c2"></a>

User satisfaction can only be measured by relevance to an information need, not by relevance to queries

## **Evaluation of Answer Sets**

### **Precision & Recall**

TODO: picture from https://en.wikipedia.org/wiki/Precision_and_recall

**Precision** is the fraction of retrieved documents that are relevant. \
_Precision_=P(relevant|retrieved)

> Can be calculated by `P = TP / (TP+FP)`

**Recall** is the fraction of relevant documents that are retrieved. \
_Recall_=P(retrieved|relevant)

> Can be calculated by `R = TP / (TP+FN)`

> TP = True Positive (for a retrieved document)\
> FP = False positive (for a retrieved document)\
> FN = False negative (for a non-retrieved document)\
> TN = True negative (for a non-retrieved document)

Trade off:

- You can increase recall by returning more documents.
- Recall is a non-decreasing function of the number of documents
  retrieved.
- A system that returns all docs has 100% recall!
- The converse is also true (usually): It’s easy to get high precision for
  very low recall.

When dealing with **ranked lists** you compute the measures for each of the returned documents, from the most relevant to the least (by their ranking; top 1, top 2, etc.) This gives you the precision-recall-curve. The **interpolation** of this result is simply to let each precision be the maximum of all future points. This removes the "jiggles" in plot, and is necessary to compute the precision at recall-level 0. A recall-level is typicall a number from 0 to 10 where 0 means recall = 0 and 10 means recall = 100 %.

Precision and recall are complementary values and should always be reported together. If reported separate it is easy to make one value arbitrarily high.

</br>

### **F-measure**

![FMeasure](./img/FMeasure.JPG)

P = precision \
R = recall

Combining precision and recall provides us with the **F-measure** (Harmonic Mean) where F symbols a good compromise between precision and recall:

We use harmonic mean because when two numbers differ greatly, the HM is closer to the minimum than their Arithmetic Mean. This will reduce the score when precision and recall differs greatly.

</br>

### **Accuracy**

The fraction of decisions (relevant/non-relevant) that are correct.

`Accuracy = (TP + TN) / (TP + FP + FN + TN)`

> Accuracy is considered a bad measurement for performance in IR models due to increased score when the number of non-relevant, non-retrieved documents increase.

## **Single Value Summaries**

There are situations in which we would like to evaluate retrieval performance over individual queries. One reason is that averaging precision over many queries might disguise important
anomalies in the retrieval algorithms under study. Another reason is that we might be interested in investigating whether a algorithm outperforms the other for each query.

### **Precision at K**

These metrics assess whether the users are getting relevant documents at the top of the ranking or not.

Precision at 5 (P@5) and at 10 (P@10) measure the precision when 5 or 10 documents have been seen.

### **Mean Average Precision: MAP**

![FMeasure](./img/MAPformula.JPG)

The idea here is to average the precision figures obtained after each new relevant document is observed.

For relevant documents not retrieved, the precision is set to 0.

**Example:**\
R<sub>q1</sub> = {d<sub>3</sub>, d<sub>5</sub>, d<sub>9</sub>, d<sub>25</sub>, d<sub>39</sub>, d<sub>44</sub>, d<sub>56</sub>, d<sub>71</sub>, d<sub>89</sub>, d<sub>123</sub>}

![FMeasure](./img/MAP1.JPG)

MAP<sub>1</sub> = (1 + 0.66 + 0.5 + 0.4 + 0.33 + 0 + 0 + 0 + 0 + 0) / 10 = 0.28

</br>

R<sub>q2</sub> = {d<sub>3</sub>, d<sub>56</sub>, d<sub>129</sub>}

![FMeasure](./img/MAP2.JPG)

MAP<sub>2</sub> = (0.33 + 0.25 + 0.20) / 3 = 0.26

MAP = (MAP<sub>1</sub> + MAP<sub>2</sub>) / 2 = (0.28 + 0.26) / 2 = 0.27

### **R-precision:**

In a search where there are R relevant documents, the R-precision is the fraction of relevant documents in the R first results. Or in other words: precision at the R-th position.

Additionally, one can also compute an average R-precision figure over a set of queries. It is however, important to be aware that this can give an imprecise score.

### **Mean Reciprocal Rank: MRR:**

<!-- https://medium.com/swlh/rank-aware-recsys-evaluation-metrics-5191bba16832 -->

Used when we're interested in finding how high in a rankinglist for a query a relevant hit is. Used when we are interested in the first correct answer to a given query or task.

TODO: Formula

### **User-Oriented Measures:**

Different users might have different relevance interpretations.

**Coverage ratio**\
Defined as the fraction of the documents known and relevant that are in the answer set.

**Novelty ratio**\
Defined as the fraction of relevant documents in the answer set that are not known to the user.

**Relative recall**\
The ratio between the number of relevant documents found and the number of relevant documents the user expected to find.

**Recall effort**\
The ratio between the number of relevant documents the user expected to find and the number of documents examined in an attempt to find the expected relevant documents.

</br></br></br>

# Relevance Feedback and Query Expansion <a name="c3"></a>

A process to improve the first formulation of a query to make it closer to the user intent. Often done iteratively (cyclic) based on feedback.

**Query Expansion (QE)**: Information related to the query used to expand it. \
**Relevance Feedback (RF)**: Information on relevant documents explicitly provided from user to a query.

A feedback cycle is composed of two basic steps:

1. Determine feedback information that is either related or expected to be related to the original query q.
2. Determine how to transform query q to take this information effectively into account.

The first step can be accomplished in two distinct ways:

- Obtain the feedback information explicitly from the users.
- Obtain the feedback information implicitly from the query results or from external sources such as a thesaurus.

## **Explicit feedback**

Feedback information provided by the users inspecting the retrieved documents.
Collecting explicit feedback information from users (in the classical way) is expensive and time consuming as the user has to examine and mark the relevant documents (in practice 10-20 top ranked are examined).

In the Web, user-clicks on search results constitute a source of feedback information from the user that the documents might be of interest in the context of the query. It does not necessarily indicate that the document is relevant to the query.

### Rocchio Method (Vector Space Model)

The Rocchito model is based on the Vector Space Model and the basic idea is to reformulate the query such that it gets:

- Closer to the neighborhood of the relevant documents in the vector
  space, and
- Away from the neighborhood of the non-relevant documents.

</br>

### Probabilistic model

The probabilistic model ranks documents for a query q according to the probabilistic ranking principle.

For the initial search (when there are no retrieved documents yet), assumptions often made include:

- P(w<sub>j</sub>|R) is constant for all terms w<sub>j</sub> (typically 0.5).
- The term probability distribution P(w<sub>j</sub>|¬R) can be approximated by the distribution in the whole collection.

The same query terms are re-weighted using feedback information provided by the user

> Notice that here, contrary to the Rocchio Method, no query expansion occurs.

The main advantage of this feedback method is the derivation of new weights for the query terms.\
The disadvantages include:

- Document term weights are not taken into account during the feedback loop.
- Weights of terms in the previous query formulations are disregarded.
- No query expansion is used (the same set of index terms in the original query is re-weighted over and over again).

Thus, this method does not in general operate as effectively as the vector modification methods.

</br>

## **Implicit feedback**

In an implicit relevance feedback cycle, the feedback information is derived implicitly by the system.

There are two basic approaches for compiling implicit feedback information:

- Local Analysis: which derives the feedback information from the top ranked documents in the result set.
- Global Analysis: which derives the feedback information from external sources such as a thesaurus.

## Local analysis

**Term-Term correlation Matrix**

TODO: same as clusters?

For classic information retrieval models, the index term weights are assumed to be mutually independent. This is however rarely the case. To take into account term-term correlations, we can compute a correlation matrix.

Let M be a term-document matrix |V| × |D|.
The matrix C = M × M<sup>T</sup> is a term-term correlation matrix.

### **Local Clustering Techniques**

**Clusters** \
Forms of deriving synonymy relationship between two local terms, building association matrices quantifying term correlations.

We consider 3 types of clusters:

**Association clusters** \
Based on the frequency of co-occurrence of terms inside documents, it does not take into account where in the document the terms occur.

The motivation is that terms that co-occur frequently inside documents have a synonymity association

**Metric clusters** \
Based on the idea that two terms occurring in the same sentence tend to be more correlated, and factor the distance between the terms in the computation of their correlation factor.

A metric cluster re-defines the correlation factors c<sub>u,v</sub> as a function of their distances in documents.

**Scalar clusters** \
Based on the idea that two terms with similar neighborhoods have some synonymy relationship, we then use this to compute the correlations.

For instance, the cosine of the angle between the two vectors is a popular scalar similarity measure.

### **Neighbor terms**

The higher the number of documents in which the terms w<sub>u</sub> and w<sub>v</sub> co-occur, the stronger this correlation. Correlation strengths can be used to define local clusters of neighbor terms. \
Terms in a same cluster is called neighbor terms and can then be used for query expansion by adding them to the query.

Query expansion is important because it tends to improve recall. It may however, come at the cost of precision as you would have a larger collection of documents.\
Thus, query expansion needs to be exercised with great care and fine tuned for the collection at hand.

### **Local Context Analysis**

Local context analysis procedure operates in three steps:

1. Retrieve the top n ranked passages using the original query.
2. For each concept c in the passages compute the similarity sim(q, c) between the whole query q and the concept c.
3. The top m ranked concepts, according to sim(q, c), are added to the original query q.

Of these three steps, the second one is the most complex and is computed as follows:

TODO: formula

> It has been adjusted for operation with TREC data and did not work so well with a different collection. It is important to have in mind that tuning might be required for operation with a different collection.

## Global analysis

Determine term similarity through a pre-computed statistical analysis on the complete collection. Expand queries with statistically most similar terms. Two methods: Similarity Thesaurus and Statistical Thesaurus. (A thesaurus provides information on synonyms and semantically related words and phrases.) Increases recall, may reduce precision.

Terms for expansion are selected based on their similarity to the whole query.

### Similarity Thesaurus

### Statistical Thesaurus

</br></br></br>

# Text Operation <a name="c4"></a>

Term selection:

Instead of using a full text representation (using all the words as index terms) we often select the terms to be used. This can be done manually, by a specialist, or automatically, by identifying noun groups. Most of the semantics in a text is carried by the noun words, but not often alone (e.g. computer science). A noun group are nouns that have a predefined maximum distance from each other in the text. (syntactic distance)

- **Normalization:** remove apostrophes, periods, and commas. Also convert to lowercase.
- **Stemming:** stem the words to their root form (Eg. Running -> Run). This might hurt precision as you might stem two non-semantically related words with the same root form. (Stemming ~ Lemmatization -> reducing to leximes).
- **Stopword removal:** Removes function words (eg. prepositions, pronouns, conjunctions). May also include single letters, digit and other common terms.

#### Lexical analysis

_Numbers_ are of little value alone and should be removed. Combinations of numbers and words could be kept. _Hyphens_ (dashes) between words should be replaced with whitespace. _Punctuation marks_ are usually removed, except when dealing with program code etc. To deal with the _case of letters_; convert all text to either upper or lower case.

#### Stopword elimination

Stopwords are words occurring in over 80% of the document collection. These are not good candidates for index terms and should be removed before indexing. (Can reduce the index by 40%) This often includes words as articles, prepositions, conjunctions. Some verbs, adverbs, adjectives. Lists of popular stopwords can be found on the Internet.

#### Stemming

The idea of stemming is to let any conjugation and plurality of a word produce the same result in a query. This improves the retrieval performance and reduce the index size. There are four types of stemming: _Affix removal, Table Lookup, Successor variety, N-grams_.
Affix removal is the most intuitive, simple and effective and is the focus of the course, with use of the _Porter Algorithm_.

#### Index term selection

Instead of using a full text representation (using all the words as index terms) we often select the terms to be used. This can be done manually, by a specialist, or automatically, by identifying _noun groups_. Most of the semantics in a text is carried by the noun words, but not often alone (e.g. _computer science_). A noun group are nouns that have a predefined maximum distance from each other in the text. (syntactic distance)

#### Thesauri

To assist a user for proper query terms you can construct a Thesauri. This provides a hierarchy that allows the broadening and narrowing of a query. It is done by creating a list of important words in a given domain. For each word provide a set of related words, derived from synonymity relationship.

### Huffman coding

Huffman coding is a prefix coding for lossless data compression, commonly used on text to compress, or code, words or characters. It assignes variable length bit-codes to a word or character (called node), where the shortest code is assigned to the most frequent node. The codes are defined by a _Huffman tree_.

#### Normal Huffman tree

A normal Huffman tree is build by sorting the nodes after frequencies. Then the two lowest nodes are joined, and their combined frequency is put back in the sorted queue. An example can be seen below:

TODO: image

The codes can be extracted from the tree by following the edges. Left edge: 0, right edge: 1.

The shortcoming of the normal Huffman tree is how it handles equal frequencies. If two nodes have the same frequency, then one of them is chosen randomly. This gives potentionally multiple, equally correct, Huffman trees from the same set of input. To cope with this we can define a _Canonical_ Huffman tree.

</br></br></br>

# Indexing and Searching <a name="c5"></a>

## Inverted Indexes

**Inverted** means that you can reconstruct the text from the index.

**Vocabulary**: the set of all different words in the text. Low space requirement, usually kept in memory.

**Occurrences**: the (position of) words in the text. More space requirement, usually kept on disk.

**Basic Inverted Index**: The oldest and most common index. Keeps track of terms; in _which_ document and _how many_ times it occur.

**Full Inverted Index**: This keeps track of the same things as the basic index, in addition to _where_ in the document the terms occurs (position).

**Block addressing** can be used to reduce space requirements. This is done by dividing text into blocks and let the occurrences point to the blocks.

**Searching** in inverted indexes are done in three steps:

1. **Vocabulary search** - words in queries are isolated and searched separately.
2. **Retrieval of occurrences** - retrieving occurrence of all the words.
3. **Manipulation of occurrences** - occurrences processed to solve phrases, proximity or Boolean operations.

**Ranked retrieval**: When dealing with _weight-sorted_ inverted lists we want the _best_ result. Sequentially searching through all the documents are time consuming, we just want the _top-k_ documents. This is trivial with a single word query; the list is already sorted and you return the first _k_-documents. For other query we need to merge the lists. (see _Persin’s algorithm_).

When **constructing** a full-text inverted index there are two sets of algorithms and methods: **Internal Algorithms** and **External Algorithms**. The difference is wether or not we can store the text and the index in internal, main memory. The former is relatively simple and low-cost, while the latter needs to write partial indexes to disk and then merge them to one index file.

In general, there are three different ways to **maintain** an inverted index:

- **Rebuild**, simple on small texts.
- **Incremental updates**, done while searching only and when needed.
- **Intermittent merge**, new documents are indexed and the new index is merged with the existing. This is, in general, the best solution.

Inverted indexes can be **compressed** in the same way as documents (chapter 6.8). Some popular coding schemes are: _Unary_, _Elias_-$\gamma$, _Elias_-$\delta$ and _Golomb_.

**Heaps’ Law** estimates the number of distinct words in a document or collection. Predicting the growth of the vocabulary size.
$V = Kn^\beta$, where _n_ is the size of the document or collection (number of words), and $10 < K < 100, 0< \beta < 1$

**Zipf’s law** estimates the distribution of words across documents in the collection (approximate model). It states that if $t_1$ is the most common word in the collection, $t_2$ the next most common, and so on, then the frequency of $f_i$ of the _i_-th most common word is proportional to $\frac{1}{i}$. That is: $f_i = \frac{c}{i}$, where _c_ is a constant. (?)

## 9.3 Signature Files

Signature files are word-oriented index structures based on hashing. It has a poorer performance than Inverted indexes, since it forces a sequential search over the index, but is suitable for small texts.

A signature file uses a **hash function** (or «signature») that maps word blocks to bit masks of a given size. The mask is obtained by bitwise OR-ing the signatures of all the words in the text block.

To **search** a signature file you hash a word to get a bit mask, then compare that mask with each bit mask of all the text blocks. Collect the candidate blocks and perform a sequential search for each.

## 9.4 Suffix Trees and Suffix Arrays

**Suffix trees**
This is a structure used to index, like the Inverted Index , when dealing with _large alphabets_ (Chinese Japanese, Korean), _agglutinating languages_ (Finnish, German).
A **Suffix trie** is an ordered tree data structure built over the suffixes of a string. A **Suffix tree** is a compressed trie. And a **Suffix array** is a «flattened» tree.
These structures handles the whole text as a string, dividing it into suffixes. Either by character or word. Each suffix is from its start point to the end of the text, making them smaller and smaller.
e.g.:

- mississippi (1)
- ississippi (2)
- …
- pi (10)
- i (11)

These structures makes it easier to search for substrings but they have large **space requirements**: A tree takes up to 20 times the space of the text and an array about 4 times the text space.



</br></br></br>

# Web Retrieval <a name="c6"></a>

</br></br></br>

# Multimedia Information Retrieval <a name="c7"></a>

</br></br></br>

## Image Retrieval <a name="c7.1"></a>

</br></br>

## Video Retrieval <a name="c7.2"></a>

</br></br>

## Audio Retrieval <a name="c7.3"></a>
