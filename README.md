# Country-HMM
Training a Hidden Markov Model to identify country references (Capital cities & world leaders too)

![image](https://github.com/DecafSunrise/Country-HMM/assets/36832027/50a423a6-5928-4fcb-aa56-72c0d2040f5a)  

## What is this?
This is an old-school model made possible by NLTK, labeling tokens (~words) as relevant to countries, or not. The intent here is kind of like NER-light, trying to identify country references.

## What's the "o" tag?
It's just a dummy tag to denote "other"

## How good is it?
![image](https://github.com/DecafSunrise/Country-HMM/assets/36832027/1b7e9a19-c430-47a0-92aa-f556ca6aec67)  
Accuracy is a fine metric, but it doesn't tell the whole story. 
- For starters, it's only a reflection of the training data. If we have flawed training data (which we do, 'cuz we're programmaticly tagging training data without human intervention), it'll make mistakes.
- To help provide a better understanding of where the model differs from the heuristic-based annotation, I'm checking a random sample of 500 article titles from my news articles corpora to see if they're a match.
  - On average, somewhere between **110-150** annotations and HMM tags differ out of the 500 **(~22-30%)** across training runs. Sometimes this is good (the model generalizes), sometimes this is bad (the model stubbornly throws errant tags).

## Where does it break?
![image](https://github.com/DecafSunrise/Country-HMM/assets/36832027/2da9d0b2-60fc-4b27-9a97-5e597aa0353f)
- Dual-meaning (polysemy) is a problem. This is clear in the example above: `Jordan` is a country, but `Jim Jordan` shouldn't be. This is somewhat to be expected from a 20+ year old model architecture, but I could probably clean it up a little with smarter annotation rules.  
![image](https://github.com/DecafSunrise/Country-HMM/assets/36832027/05e89c35-18bd-46f3-9117-57a75449ceed)  
- The model gets a little tag-happy with the beginnings of sentences. Many article titles begin with "Country: Words words words", so it's possible it's over-fitting on that pattern. I've baked in a validation step for the country ISO names so that the initial training corpus doesn't include it, but I augment (read: Dwarf) this corpus with 50k randomly selected articles right after, without that validation step. 

## What's next?
- Consider adding additional classes, as it may provide additional context to the model
- Look into ways to use Part of Speech (POS) tags to decrease false-positives in training data. Country references should often be special cases of proper nouns.
