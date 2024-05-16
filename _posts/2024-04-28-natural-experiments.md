---
layout: post
title: "Natural Experiments in WSJ"
categories: ["short posts", "econometrics"]
---

I read an [article](https://www.wsj.com/business/energy-oil/in-americas-biggest-oil-field-the-ground-is-swelling-and-buckling-9d66eb42?mod=hp_lead_pos9) this weekend in the Wall Street Journal that reminded me of the interesting way that natural experiments can arise. 

In discussing the rapid growth of oil extraction in the Delaware Basin - spanning the border between New Mexico and Texas - the article highlights the diverging land subsidence and expansion across the geography, as oil and water are extracted from some areas and wastewater is pumped into the ground in others. That process has had notable effects on local communities. In particular, the regions of Texas that have experienced rising surface levels have also seen a significant expansion in earthquakes, in an area that registered few notable quakes in the early part of the 2010s.

The screenshot below highlights the stark disparity in wastewater disposal volumes on either side of the New Mexico-Texas border, and one of the explanations given is the much looser regulatory constraints imposed in Texas. In recent years many firms have taken to extracing wastewater from wells in New Mexico and shipping it over to Texas for disposal.

![Water disposal in the Delaware Basin](/docs/assets/images/wsj-article-permian-apr2024.png)Source: WSJ

The (at least visibly) pretty clear difference in deposited volumes across the state line _could_ be due to a number of factors (geology, cost, etc.) but one thing we can suggest as a reason is the difference in regulation on either side of the border. Clear borders of some kind (in this case regulatory) between otherwise similar areas can be a really interesting tool for analyzing the effects of interventions on different groups. 

Specifically, this all reminded me of a 1994 [paper](https://davidcard.berkeley.edu/papers/njmin-aer.pdf) by Card & Krueger studying the effects of minimum wage increases on employment. They use the different regulations (in this case the minimum wage) between New Jersey and Pennsylvania as a tool to examine how an increase in minimum wage in New Jersey (but not in Pennsylvania) changed employment in that state. Their finding (which was and remains a topic of debate) was that the increase in minimum wage in the treatment state (New Jersey) did not result in a decrease in employment relative to the control state (Pennsylvania). It was, I believe, one of the early difference-in-difference papers that really caught the attention of economists - in particular because the results ran contrary to what labor models would suggest.

(There's a seminar discussion of the paper [here](https://www.eco.uc3m.es/docencia/EconomiaAplicada/materiales/CardKrueger94_en.pdf), along with many other discussions.)

Now, I have no knowledge of either earthquakes or oil and gas extraction. So whether there is any question that this kind of natural experiment can answer is for someone else to judge. But it's nice to be able to visualize the kind of 'line on a map' differences that present opportunities to conduct experiments on observational data.

_As usual, please reach out with corrections or comments to feedback@finlaymcalpine.com_