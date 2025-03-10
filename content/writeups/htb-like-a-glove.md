+++
date = '2025-03-10T08:32:03-05:00'
title = 'HTB - Like A Glove'
categories = ["Hack The Box", "CTF"]
tags = ["python", "AI-ML"]
showTaxonomies = true
showTableOfContents = true
+++

## Challenge description
{{< lead >}}
Words carry semantic information. Similar to how people can infer meaning based on a word's context, AI can derive representations for words based on their context too! However, the kinds of meaning that a model uses may not match ours. We've found a pair of AIs speaking in metaphors that we can't make any sense of! The embedding model is glove-twitter-25. Note that the flag should be fully ASCII ans starts with 'htb{'.
{{< /lead >}}

## Provided files
### chal.txt

```
Like non-mainstream is to efl, battery-powered is to?
Like sycophancy is to بالشهادة, cont is to?
Like беспощадно is to indépendance, rs is to?
Like ajaajjajaja is to hahahahahahahahaahah, ２ is to?
Like bahno is to arbus, duit is to?
Like 잡히지 is to ਮੈਂ, 年度第 is to?
Like usaar is to est-ce-que, ７ is to?
Like وشعب is to nordeste, rêve is to?
Like teapots is to glow, ４１ is to?
Like run-ins is to biggy, update is to?
Like curioso is to uluh, musical is to?
Like descended is to moonwalker, ` is to?
Like kitai is to nhahahhaha, ３ is to?
Like volcanos is to dwd, real is to?
Like no-nonsense is to inventor, ツ is to?
Like kirleten is to sur-norte, tre is to?
Like lelel is to overburen, ３ is to?
Like whtevr is to woooooooooow, revlon is to?
Like gusher is to fysieke, ８ is to?
Like ものかげから is to flare, ske is to?
Like nhahahahahaha is to pep-rally, ０１０ is to?
Like getoond is to menor, ons is to?
Like htb is to provincias, ３０ is to?
Like dénonce is to pregonando, ｒ is to?
Like literally is to built, ! is to?
Like invisable is to laking, ` is to?
Like sergent is to borma, mvp is to?
Like energiekosten is to auditeur, u is to?
Like achtung is to andı, ＞ is to?
Like theologians is to saldrán, does is to?
Like imposing is to amara, | is to?
Like aeriel is to koysa, ９ is to?
Like desencadena is to importer, hace is to?
Like freestylers is to sanggah, temperature is to?
Like neverhaveiever is to judibana, お互い才能を発揮しましょ is to?
Like delain is to omán, dy is to?
Like watered-down is to viejitoo, | is to?
Like ahk is to meitat, ≡ is to?
Like mags is to wiza, r is to?
Like multi-coloured is to cendere, l is to?
Like hyping is to nibbler, ai is to?
Like siddharth is to davno, ／ is to?
Like jaajaajaa is to checkyourself, b is to?
Like good. is to watis, － is to?
Like هيتش is to puuees, flag is to?
Like verslaggever is to manada, ′ is to?
Like timelords is to وتحرر, pisałaś is to?
Like переменах is to предвкушение, x is to?
Like espertas is to uhahuahuahu, ４ is to?
Like wannabes is to peaceofmind, rt is to?
Like mindshare is to stackers, ２n is to?
Like laxative is to succession, __ is to?
Like xb is to mlt, v is to?
Like jesuit is to rose, ３３ is to?
Like sentience is to charles., removing is to?
Like админов is to планировки, ハイ is to?
Like culebron is to lovecurling, mama is to?
Like prophethood is to bookend, ＝ is to?
Like coincidentemente is to longsleeves, ´ is to?
Like brookyln is to magistra, j is to?
Like theoffice is to notrealtime, ２ is to?
Like presupone is to viewport, se is to?
Like walklikeus is to benefic, ＃ is to?
Like deer is to baddi, witch is to?
Like destructing is to khleo, ̮ is to?
Like зафолловить is to eqs, 丿 is to?
Like anette is to griefers, l is to?
Like surfs is to smocking, ) is to?
Like petrolio is to gradual, m is to?
Like roguelike is to brainfreeze, ２ is to?
Like aromáticas is to macuspana, mananitas is to?
Like call-up is to ahahahahahahahahahahahahahahahahahahahaha, ６ is to?
Like top-up is to pose, aud is to?
Like century-old is to débs, » is to?
Like gofast is to paracetamol, 高校野球 is to?
Like troubleshooter is to theonethatgotaway, v is to?
Like jajajjjajajaja is to carbohidrato, ⤴ is to?
Like jotaro is to powerboat, ３ is to?
Like t'écoutes is to pdv, fans is to?
Like raveology is to nikla, tωt is to?
Like rehashed is to zakri, qualification is to?
Like dicipline is to prosecuted, نوصلها is to?
Like low-cost is to we'on, file is to?
Like raving is to سگن, happy is to?
```

## Solution
Googling "glove-twitter-25", you can find two interesting websites: [the GloVe algorithm website](https://nlp.stanford.edu/projects/glove/), and [an introduction to the Gensim Python library](https://kavita-ganesan.com/easily-access-pre-trained-word-embeddings-with-gensim/). 

I'm no expert in Machine Learning, so take what I'm saying with a grain of salt. From my understanding, words are just vectors to AI. Just like vectors, they have dimensions and values for each dimension and you can do the usual operations on them and between them. GloVe can obtain the vector representation of words based on the context that surounds them, obtaining results such as related words being close to each other in the vector space (like "frog" and "rana"), and their opposites having roughly the same distance between them. For example, the distance between "man" and "woman" is approximately the same as the distance between "king" and "queen" (if what you're reading seems too complex, you can check out a few basic Linear Algebra courses to understand vectors and vector spaces). In this challenge, GloVe has been trained with tweets (or posts) from Twitter (or X) to obtain the vector representation of a set of words. As you can see, they don't even have to be words, any symbol can be represented as a vector!

{{< katex >}}
With this idea, we can draw a sketch of how to solve this challenge. If we think about words just as vectors, then an analogy between two pairs of words is like two pairs of vectors that have roughly the same distance between them. In other words, we can think of the first analogy like \\(\text{non-mainstream} - \text{efl} = \text{battery-powered} - \vec{w}\\). Then, we'd just have to solve for the vector \\(\vec{w}\\) and find the closest word to it in the vector space (since the relationship in analogies are approximante, we can't guarantee that the vector \\(\vec{w}\\) will represent a word with exact operations, so we need to search for the closest word to it for the analogy to work). All of this can be easily achievable with the Gensim python library.

In the introduction to Gensim I've linked previously, you can see loading and using the model used in this challenge is straightforward. What's interesting is the [`most_similar` function](https://radimrehurek.com/gensim/models/keyedvectors.html#gensim.models.keyedvectors.WordEmbeddingsKeyedVectors.most_similar), which returns the most similar words to a list of words or vectors. With this, we can find the most similar word to the vector \\(\vec{w}\\). Now we can start writing a script to solve this challenge.

### Script

``` python
#!/usr/bin/env python3

import gensim.downloader as api

model_glove_twitter = api.load("glove-twitter-25")


def find_analogy(x, y, a):
    # Accessing the vector representation of the words
    vec_x = model_glove_twitter[x]
    vec_y = model_glove_twitter[y]
    vec_a = model_glove_twitter[a]

    result = model_glove_twitter.most_similar([vec_a - (vec_x - vec_y)])

    most_similar_key, similarity = result[0]

    return most_similar_key


sols = []
with open("chal.txt", "r") as file:
    for line in file:
        words = line.split()
        sols.append(find_analogy(words[1], (words[4])[:-1], words[5]))
solution = "".join(sols)
print(solution)
```

## Partial flag

``` 
htb{h４rm０n１ou５_hymn_０f_h１ghd１m３ns１０n４l_sub...
```

As you can see, the numbers are a bit wider than what you normally see. It seems like the model uses the fullwidth representation of numbers instead of the ASCII one. For the flag to be accepted, as the challenge description says, it needs to be in ASCII format. You can modify the script to do this, but any chatbot will happily do it for you.
