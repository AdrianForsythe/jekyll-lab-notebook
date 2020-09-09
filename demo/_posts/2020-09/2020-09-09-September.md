---
title: '9-9-20 Lab Notes'
output:
  html_document: default
  pdf_document: default
---

# What am I working on today:
 - Setting up eLabFTW on a DigitalOcean Droplette
    - accessible through https://104.248.33.207/login.php
 - Keeping track of everything I do through markdown notebooks!
 - Going through list of samples we have and thinking of ideas for reserch projects
 - Drafting Individual Development Plan
 - Finishing Thesis Intro Chapter, starting Conclusion re-write?
    - send to Sarah for edits?
 - Send qPCR instructions to Himeshi
 - Send sample spreadsheet to JP
    - send sample prep instructions to Greg
 
 
## Dental Calcus Samples

We have this set of dental calcus samples from a collection of different species, locations, and times. This is a list of all the samples that have been processed in this lab, and whether or not they have been extracted, prepped, and sequenced.

```{r,warning=FALSE,error=FALSE,message=FALSE}
library(tidyverse)
library(lubridate)
sample.list<-readxl::read_xlsx("Sample_processing_masterlist.xlsx") %>% as.data.frame()
```

Only one polar bear sample! The rest are brown bears. Consider these bears, how many samples do we have that have been sequenced?
```{r}
sample.list %>% 
  filter(Spec.genus == "Ursus") %>% 
  pivot_longer(SAMPLED:SEQUENCED,names_to="state",values_to="status") %>%  
  group_by(state) %>% 
  mutate(status=ifelse(status=="Yes",1,0)) %>%
  summarise(n=sum(status[status==1],na.rm = T)) %>% print()
```

Of these sequenced samples, what's the distribution of the sampling year look like?
```{r}
sample.list %>% 
  filter(Spec.genus == "Ursus" & SEQUENCED == "Yes") %>%
  ggplot(aes(x=Spec.coll.year))+geom_histogram()+geom_vline(xintercept = 1954)
```

For a start, Katja mentioned that Bears seem to have a lot of dental caries
```{r}
sample.list %>% 
  filter(Spec.genus == "Ursus" & SEQUENCED == "Yes") %>% 
  mutate(time.period = ifelse(Spec.coll.year>(1945),"modern","ancient")) %>% 
  ggplot(aes(x=Bear.PoorOralHealth,fill=Bear.PoorOralHealth))+
    geom_bar(stat="count",color="black")+
    facet_grid(time.period~.)+
    scale_x_discrete(labels=c("Healthy","Unhealthy"))+theme(legend.position = "none")
```

## Ideas for Potential Analyses

### Bear Oral Health

#### Prevalance of dental caries

#### Prevalance of dental caries
**In Europe**

- Stromquis et al 2009,"There was a low prevalence of calculus and periodontal disease and none of the bears had caries infections"
  - The bears studied here did not eat honey (Dahleet al., 1998)

**In North America**

- Some prevalence in North American populations
  - No obvious signs of caries in North American brown bears.
  - Manville (1992) described a prevalence of 10.5% in black bears from Wisconsin (n=86) 20% in black bears from Michigan (n=35). 
    - Hall (1940) low prevalence in black bears (3%,n=195) and grizzly (2%,n=165) bears
    - alkaline pH of the saliva (mean 9.75) saliva, may be one explanation for the low frequency of caries?

#### Cause of dental caries

**Human Influence**

  - Manville (1992) described a prevalence of 10.5% in black bears from Wisconsin (n=86) 20% in black bears from Michigan (n=35). Hall (1940)
    - low prevalence in black bears (3%,n=195) and grizzly (2%,n=165)bears
    - saliva alkaline pH (mean 9.75) may be one explanation for the low frequency of caries.

#### Cause of dental caries

**Human Influence**

- brown bears do approach human settlements, predate livestock and occasionally consume human-associated plant items like cereals
- Direct human contact can also occur, primarily with hunters and their hunting dogs
- Free-ranging vs. In captivity
  - Captive bears had a higher prevalence of calculus and caries than free-ranging bears
    - due to inappropriate diet and lack of tooth-cleaning activities (?)
  - Dental caries increase with age in brown bears, and are in higher incidence than free-range bears
  - Bernese bear pit: severe dental health problems warranting treatment, 1850 and 1995. 

**Diet**

- carbohydrate-rich diet of berries and honey was the cause of caries.
- a nutritionally appropriate diet has a self-cleaning effect (?)
  - **what microbes are associated with healthy/non-healthy teeth**


### Analyses
- test pathogens in the background of metagenomic datasets, using HOPS? (Hübler et al 2019)
  - modified version of MALT, including PCR duplicate removal and deamination pattern tolerance
  - MaltExtract filters for read length, sequence complexity, or percent identity. Authenticate the presence of species-specific aDNA
  - post-processing, summarising all samples and potential bacterial pathogens that have been identified.
  - authenticated four: *Clostridium tetani* (soil), *Streptococcus mutans* (calculus, dentine), *Treponema denticola* (calculus, dentine), and *Porphyromonas gingivalis* (calculus only)
  - identification likely reflects true positives.

<!-- ![Caption]("~/Downloads/HOPS.png") -->

### Questions

1. What other factors of the bear diet are attributed to poor oral health (i.e., carb-rich)
  i) if not honey, then what else is natural?
  ii) if not natural, then where, when, how?
2. Can we associate host community shift with presence/absence of known oral pathogens? Temporally as well?
3. Can we associate host community shift with evidence of dietary changes? Again, temporally?
3. Are there enough sequenced samples to address these questions?
4. Exclusion of samples based on location, demographics, etc.?

# What do I need to work on tomorrow:

- Sequenced/non-sequenced samples and caries vs. non-carries samples.
- Scoring bear disease prevalance and population levels
- email Jeala
  - Oral microbiome signature, sourcetracker, samples that haven't sequenced well

# What do I need to work on for next week:
1) Zoologiska
2) IDP
3) Paper Review
