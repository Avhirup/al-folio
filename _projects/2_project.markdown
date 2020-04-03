---
layout: page
title: Query Understanding
description: Query Understanding Module for Embibe Search
img: 
published: true

---

**Query understanding** is the process of inferring the intent of a user by extracting semantic meaning from the searcher’s keywords. At Embibe, students use search to either learn a new topic, examine their skills about a topic or find articles related to a topic. To serve students better, we classify their intent to LEARN, EXAMINE, or INFO. We also find entities from their query to refine our search space.

For the query **“inverse of matrix practice”**, the user wants to test their skills on the chapter of *“the inverse of a matrix”*. Therefore, the intent is to *EXAMINE* & detected entities are (“inverse of a matrix”, chapter).

Below mentioned are a collection of modules, each of which would be discussed breifly later:
* Spell Correction
* Query Rewrite
* Intent Detection
* Academic Entity Recognition


### Spell Correction
Our spell correction solution based out of use of distribution of unigrams and bigrams in education corpus. We use the [SymSpell](https://github.com/wolfgarbe/SymSpell) to build our spell correction. [Peter Norvig's blog](https://norvig.com/spell-correct.html) is a great resource on how we are using these n-grams.

### Query Rewrite
Query Rewrite module is responsible for altering user query to solve vocabulary mismatch problem. This solution is based out of static config, where we group the changes into:
* abbreviations (shm → simple harmonic motion)
* synonyms (class 8 → 8th Foundation)
* chemical compound to their names (k2cr2o7 → potassium dichromate)
* common misspells (jee mains → jee main)

### Intent Detection
At Embibe, students visit the homepage and are presented with the option to search for anything “academic”. This could be from the chapter they missed in school and what to learn about it, or to practice the mathematical problems about trigonometry, to getting the latest news about upcoming competitive exams. These are called search query intent.

We can categorize these search queries mainly into 3 intent:
* **LEARN** : These search queries imply that a student wants to learn a topic. These topics could include exam, chapter, unit, subject, etc. 
    - “ learn matrices ”
    - “ newtons law of motion”
* **EXAMINE** : These search queries imply that a student wants to examine their knowledge on a topic. These could be in the form of taking tests or practice problem.
    - “Practice problems on inorganic chemistry”
    - “Sums on integration”
* **INFO** : These search queries imply that a student wants to read articles on a topic. These could be in the form of articles on cutoff, tips and tricks, latest news, etc.
    - “Mathematics tips and tricks”
    - “Latest news on railway exam on 2019” 


We used AWD-LSTM based language, which is pre-trained on Education Corpus(Books, Questions, Wikipedia). This helps use learn a good representation for academic entities. This learnt representation could be used not only for Intent Detection but also for *Query Expansion* , *Academic Entity Recognition* etc.   

### Academic Entity Recognition
Academic Entity Recognition is a NER task. At Embibe,these entities could belong to either exam, subject, units, chapters, concepts, date. These entities can later be used to refine search space. 
We use a hybrid solution of Regex Based tagger & ML based tagger. We follow a similar solution to [Expedia](https://www.semanticscholar.org/paper/Named-Entity-Recognition-in-Travel-Related-Search-Cowan-Zethelius/2da40f5dda818aea7cca17affa976735c0452cb6) but instead of using CRF, we use our pretrained language model and fine tune it on search queries at academic entity detection task.


Feel free to reach out to me for any questions.



