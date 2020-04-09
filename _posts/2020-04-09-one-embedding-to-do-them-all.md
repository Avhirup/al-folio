---
layout: post
title: One Embedding to do them all
date: 2020-04-05 16:12:00-0400
description: A framework to combine multi datasources to a unified embedding for products on e-commerce. These datasource could include catalog text data, user click stream data or product images.
published: true
tags: [paper summary]
---


### TLDR 
A weighted sum of different embedding from multiple data sources(catalog, click stream and product image); each trained on different techniques ranging from denoising autoencoders for text, Bayesian personalised ranking(BPR) for clickstream, siamese network trained for images.

[Link to the paper](https://arxiv.org/abs/1906.12120)

### Introduction
In Ecommerce, we often encounter problem when we would want to represent product using a single embedding. These embedding could be used for different ecommerce tasks which could involve specifically checking product attribute coverage, finding similar products and predicting returns. This unified embedding could be built from different data source, each capturing a different sense of representation. The different data sources include 
* Textual Data : This involves products’ title (name), description and cataloged attributes like brand, color, fabric and physical attributes like neck, pattern etc.
* Clickstream Data : This includes all the users’ ses- sions and the involved interactions including searches, impressions, clicks, sorts and, filters used, add to carts, purchases etc.
* Visual Data : This includes product images available in the catalog.

### Problem Statement
Generate product embedding using data from multiple data sources.

### Methodology 
A seperate embedding technique is used for different data source. A weighted sum of these embeddings is assigned as the product embedding. This single product embedding is later evaluated on multiple ecommerce tasks ; namely 
* Embedding to Attribute
* Clicked-Purchased Product Similarity
* Cart Return Prediction
<div class="img_row">
    <img class="col three" src="{{ site.baseurl }}/assets/img/one-embedding-to-do-them-all/various_techniques.png" width="719" height="308">
</div>
Below mentioned are couple of techniques for building embedding:
* Bayesian Personalised Ranking - MF (ClickStream)
	* Product embeddings are learned using Matrix Factorisation for modeling implicit feedback and Bayesian Personalised Ranking as a pairwise ranking optimization method.The objective is for each i, j product pair for which user u has given positive implicit feedback about product i and not observed product j
	$$ x_{u, p}=\alpha+\beta_{u}+\beta_{p}+\gamma_{u}^{T} \gamma_{p} $$
	$$ \sum_{\vee(u, i, j)} \log \left(\sigma\left(x_{u, i}-x_{u, j}\right)\right)-\lambda_{\theta}\|\theta\|^{2} $$
* Denoising AutoEncoder  (Side Information)
	* We train a Denoising AutoEncoder for a pure content- based recommendation setting. All side information of products is one hot encoded and fed into a denoising autoencoder. Denoising AutoEncoder minimizes the reconstruction error between corrupted input and reconstructed input,as follows $$
J(\theta)=\frac{1}{2|P|} \sum_{i=1}^{i=|P|}\left\|x_{i}-\operatorname{Dec}\left(\operatorname{Enc}\left(x_{i \operatorname{corr}}\right)\right)\right\|^{2}
$$
* Image Embeddings 
	* The deep convolution neural network architecture learned using a triplet loss has been specifically trained to learn fine-grained image similarity.
* Word2Vec Based Embeddings
	* A skipgram model is built on co-purchased and co-bagged(added to bag) data from a user’s lifetime history. 

### Results 
<div class="img_row">
    <img class="col three" src="{{ site.baseurl }}/assets/img/one-embedding-to-do-them-all/Results.png" width="719" height="308">
</div>
