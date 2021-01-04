---
layout: post
comments: false
title: Pubmed Search project
categories: [Personal_Projects]
date: 2021-01-04
---

This script sends a query to pubmed. It returns a text file with the results of the query and the corresponding
information about the papers. For now it returns Pubmed-id, title, abstract and citation.

```
#!/usr/bin/python3
##################
# Usage: pubmed-example.py > results.out
########################################
# This script stores information from the
# papers found in pubmed
########################################

from pymed import PubMed
import subprocess
from multiprocessing import Pool

pubmed = PubMed(tool="SearchTool", email="julio.rodino@ug.uchile.cl")
results = pubmed.query("EEG alpha bistable perception", max_results = 6)

def get_citation(article):

        pubmed_id = article.pubmed_id.splitlines()
        citation = subprocess.run(args = f"pubmed-citation {pubmed_id[0]}", shell = True, stdout = subprocess.PIPE)

        print(f"[Pubmed_id]: {pubmed_id[0]}")
        print(f"[Title]: {article.title}")
        print(f"[Abstract]: {article.abstract}")
        print(f"[Citation]: {citation.stdout}")
        print("")

p = Pool(3)
p.map(get_citation, results)
p.close()
p.join()

exit()
```
