---
layout: page
title: Introduction
permalink: /introduction/
---

## Mining and Utilizing Dataset Relevancy from Oceanographic Dataset (MUDROD) Metadata, Usage Metrics, and User Feedback to Improve Data Discovery and Access

*Bookmark this page!*

**Background and goal**: Massive amounts of geospatial datasets are archived and made available through online web discovery and access methods. However, finding the right data for scientific research and application development is still a challenge. We propose to mine and utilize the combination of Earth Science dataset metadata, usage metrics, and user feedback to objectively extract relevance for improved data discovery and access across a NASA Distributed Active Archive Center (DAAC) and other data centers. As a point of reference, the Physical Oceanographic Distributed Active Archive Center (PO.DAAC) aims to provide datasets to facilitate scientists in selecting Earth observation data that fit better their needs in various aspects of Physical Oceanography.

**What are the problems in Earth data discovery?**

* **Only keyword-matching, no semantic context**： If a user searches for “sea surface temperature,” it is understood by traditional search engine as “sea AND surface AND temperature,” but the real intent of this user might be “sea surface temperature” OR “sst” OR “ghrsst” OR …

* **Single-dimension based ranking**： There are typically hundreds or even thousands of search hits when you search by a keyword. Many be websites provides some sorts of metrics (e.g., spatial resolution, processing level) to help sort the search hits. It can be helpful sometimes, but it also induces users to just focus on one single data characteristic. What if a user is looking for a data that has both high spatial resolution and processing level?

* **Lack of data-to-data relevancy**： In theory, there exist hidden relationships among the data hosted within a data center or across different data centers. In reality, we can only view a particular data after clicking on it without knowing its related data.


**How does MUDROD address them?**

<center>
	<img src="/images/architecture.jpg">
	Figure 1. System architecture
</center>

* **Query expansion and suggestion**： Rather than manually create a domain ontology, MUDROD discovers the latent semantic relationship through analyzing the web logs and metadata documents. The process could be thought of as a type statistical analysis. The hypothesis for user behavior analysis is that similar queries result in similar clicking behaviors. The intuition is that if two queries are similar, the clicked data will be similar in the context of large-scale user behaviors. The analysis results would be the similarities between any pair of data. For example, we found that the similarity between “ocean temperature” and “sea surface temperature” is nearly one, meaning exactly the same. These similarities would be very helpful in terms of better understanding users’ search intent. For example, an original query “ocean temperature” can be expanded to “ocean temperature (1.0) OR sea surface temperature (1.0)”. This converted query has been proven to be able to improve both the search recall and precision

* **Learning to ranking**： After discussing with domain experts, we identified about eleven features that can reflect users’ search interests. These features primarily come from three aspects, including user behavior, query-metadata overlap, metadata attributes. From there, we trained a machine learning ranking algorithm with human-labelled query results. The reason we use machine learning here is that it is difficult to weight these features, especially the number of them can be larger down the road. After that, we use the machine learned model to predict the results of other unseen queries.

* **Recommendation**： To find the relatedness of different data, two types of information are considered, user behavior and metadata attributes. In terms of the metadata attributes, it is straightforward that we just need to compare the text of the metadata documents. For the user behavior based recommendation, we just need to find the most similar user(s) to you, and then find the data that have been clicked by your most similar user(s) but you haven’t. We can get the best recommendation for us by merging the results from these two methods.