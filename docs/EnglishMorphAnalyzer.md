# English Morphological Analyzer

English Morphological Analyzer takes a list of tokens and a list of corresponding part-of-speech tags in the Penn Treebank style, and returns a list of their base forms (lemmas):
```python
from elit_morph_analyzer import EnglishMorphAnalyzer

analyzer = EnglishMorphAnalyzer()

tokens = ['I', 'bought', 'books', 'for', 'studying', 'earlier']
postags = ['PRP', 'VBD', 'NNS', 'IN', 'VBG', 'JJR']
lemmas = analyzer.decode(tokens, postags) 
```

Output:
```python
['i', 'buy', 'book', 'for', 'study', 'early']
```

By default, it uses only inflection rules to analyze morphemes.
If you set `baseonly=False`, it returns the lemmas as well as morpheme tags (see the tagsets below):
```python
tp = []

# verb: 3rd-person singular
tp.append((['pushes', 'pushes', 'takes'], ['VBZ', 'VBZ', 'VBZ']))

# verb: gerund
tp.append((['lying', 'feeling', 'taking', 'running'], ['VBG', 'VBG', 'VBG', 'VBG']))

# past (participle)
tp.append((['denied', 'zipped', 'heard', 'written', 'clung'], ['VBD', 'VBD', 'VBD', 'VBN', 'VBN']))

# verb: irregular
tp.append((['bit', 'bound', 'took', 'slept', 'spoken'], ['VBD', 'VBD', 'VBD', 'VBD', 'VBN']))

# noun: plural
tp.append((['crosses', 'men', 'vertebrae', 'foci', 'optima'], ['NNS', 'NNS', 'NNS', 'NNS', 'NNS']))

# noun: irregular
tp.append((['indices', 'wolves', 'knives', 'quizzes'], ['NNS', 'NNS', 'NNS', 'NNS']))

# adjective: comparative, superative
tp.append((['easier', 'larger', 'smallest', 'biggest'], ['JJR', 'JJR', 'JJS', 'JJS']))

# adverb: comparative, superative
tp.append((['earlier', 'sooner', 'largest'], ['RBR', 'RBR', 'JJS']))

# adjective/adverb: irregular
tp.append((['worse', 'further', 'best'], ['JJR', 'RBR', 'JJS']))

for tokens, postags in tp:
    print(analyzer.decode(tokens, postags, baseonly=False))
```

Output:
```python
[[('push', 'VB'), ('+es', 'I_3PS')], [('push', 'VB'), ('+es', 'I_3PS')], [('take', 'VB'), ('+s', 'I_3PS')]]
[[('lie', 'VB'), ('+ying', 'I_GRD')], [('feel', 'VB'), ('+ing', 'I_GRD')], [('take', 'VB'), ('+ing', 'I_GRD')], [('run', 'VB'), ('+ing', 'I_GRD')]]
[[('deny', 'VB'), ('+ied', 'I_PST')], [('zip', 'VB'), ('+ed', 'I_PST')], [('hear', 'VB'), ('+d', 'I_PST')], [('write', 'VB'), ('+en', 'I_PST')], [('cling', 'VB'), ('+ung', 'I_PST')]]
[[('bite', 'VB'), ('-e', 'I_PST')], [('bind', 'VB'), ('+ou+', 'I_PST')], [('take', 'VB'), ('+ook', 'I_PST')], [('sleep', 'VB'), ('+pt', 'I_PST')], [('speak', 'VB'), ('+oken', 'I_PST')]]
[[('cross', 'NN'), ('+es', 'I_PLR')], [('man', 'NN'), ('+men', 'I_PLR')], [('vertebra', 'NN'), ('+ae', 'I_PLR')], [('focus', 'NN'), ('+i', 'I_PLR')], [('optimum', 'NN'), ('+a', 'I_PLR')]]
[[('index', 'NN'), ('+ices', 'I_PLR')], [('wolf', 'NN'), ('+ves', 'I_PLR')], [('knife', 'NN'), ('+ves', 'I_PLR')], [('quiz', 'NN'), ('+es', 'I_PLR')]]
[[('easy', 'JJ'), ('+ier', 'I_COM')], [('large', 'JJ'), ('+er', 'I_COM')], [('small', 'JJ'), ('+est', 'I_SUP')], [('big', 'JJ'), ('+est', 'I_SUP')]]
[[('early', 'RB'), ('+ier', 'I_COM')], [('soon', 'RB'), ('+er', 'I_COM')], [('large', 'JJ'), ('+est', 'I_SUP')]]
[[('bad', 'JJ'), ('', 'I_COM')], [('far', 'RB'), ('+urthe+', 'I_COM')], [('good', 'JJ'), ('', 'I_SUP')]]
```

If you set `derivation=True`, it uses both inflection and derivation rules for the analysis.
The followings show examples of verb derivations: 
```python
tp = []

# verb: 'fy'
tp.append((['glorify', 'qualify', 'simplify', 'liquefy'], ['VB', 'VB', 'VB', 'VB']))

# verb: 'ize'
tp.append((['hospitalize', 'theorize', 'crystallize', 'dramatize'], ['VB', 'VB', 'VB', 'VB']))

# verb: 'en'
tp.append((['strengthen', 'brighten'], ['VB', 'VB']))

for tokens, postags in tp:
    print(analyzer.decode(tokens, postags, derivation=True, baseonly=False))
```

Noun derivations:
```python
tp = []

# noun: 'age'
tp.append((['marriage', 'passage', 'mileage'], ['NN', 'NN', 'NN']))

# noun: 'al'
tp.append((['denial', 'approval'], ['NN', 'NN']))

# noun: 'ance'
tp.append((['annoyance', 'insurance', 'relevance', 'difference', 'accuracy'], ['NN', 'NN', 'NN', 'NN', 'NN']))

# noun: 'ant'
tp.append((['applicant', 'assistant', 'servant', 'immigrant', 'resident'], ['NN', 'NN', 'NN', 'NN', 'NN']))

# noun: 'dom'
tp.append((['freedom', 'kingdom'], ['NN', 'NN']))

# noun: 'ee'
tp.append((['employee', 'escapee'], ['NN', 'NN']))

# noun: 'er'
tp.append((['carrier', 'lawyer', 'writer', 'liar', 'actor'], ['NN', 'NN', 'NN', 'NN', 'NN']))

# noun: 'hood'
tp.append((['likelihood', 'childhood'], ['NN', 'NN']))

# adjective: 'ing'
tp.append((['building'], ['NN']))

# noun: 'ism'
tp.append((['witticism', 'baptism', 'capitalism', 'bimetallism'], ['NN', 'NN', 'NN', 'NN']))

# noun: 'ist'
tp.append((['capitalist', 'machinist', 'panellist', 'environmentalist'], ['NN', 'NN', 'NN', 'NN']))

# noun: 'ity'
tp.append((['capability', 'variety', 'normality', 'loyalty'], ['NN', 'NN', 'NN', 'NN']))

# noun: 'man'
tp.append((['chairman', 'chairwoman', 'chairperson'], ['NN', 'NN', 'NN']))

# noun: 'ment'
tp.append((['development', 'abridgment'], ['NN', 'NN']))

# noun: 'ness'
tp.append((['happiness', 'kindness', 'thinness'], ['NN', 'NN', 'NN']))

# noun: 'ship'
tp.append((['friendship'], ['NN']))

# noun: 'sis'
tp.append((['diagnosis', 'analysis'], ['NN', 'NN']))

# noun: 'tion'
tp.append((['verification', 'admiration', 'suspicion', 'addition', 'decision'], ['NN', 'NN', 'NN', 'NN', 'NN']))

for tokens, postags in tp:
    print(analyzer.decode(tokens, postags, derivation=True, baseonly=False))
```

Output:
```python
[[('marry', 'VB'), ('+iage', 'N_AGE')], [('pass', 'VB'), ('+age', 'N_AGE')], [('mile', 'NN'), ('+age', 'N_AGE')]]
[[('deny', 'VB'), ('+ial', 'N_AL')], [('approve', 'VB'), ('+al', 'N_AL')]]
[[('annoy', 'VB'), ('+ance', 'N_ANCE')], [('insure', 'VB'), ('+ance', 'N_ANCE')], [('relevant', 'JJ'), ('+ance', 'N_ANCE')], [('differ', 'VB'), ('+ent', 'J_ANT'), ('+ence', 'N_ANCE')], [('accurate', 'JJ'), ('+cy', 'N_ANCE')]]
[[('apply', 'VB'), ('+icant', 'N_ANT')], [('assist', 'VB'), ('+ant', 'N_ANT')], [('serve', 'VB'), ('+ant', 'N_ANT')], [('immigrate', 'VB'), ('+ant', 'N_ANT')], [('reside', 'VB'), ('+ent', 'N_ANT')]]
[[('free', 'JJ'), ('+dom', 'N_DOM')], [('king', 'NN'), ('+dom', 'N_DOM')]]
[[('employ', 'VB'), ('+ee', 'N_EE')], [('escape', 'VB'), ('+ee', 'N_EE')]]
[[('carry', 'VB'), ('+ier', 'N_ER')], [('law', 'NN'), ('+yer', 'N_ER')], [('write', 'VB'), ('+er', 'N_ER')], [('lie', 'VB'), ('+ar', 'N_ER')], [('act', 'VB'), ('+or', 'N_ER')]]
[[('like', 'NN'), ('+ly', 'J_LY'), ('+ihood', 'N_HOOD')], [('child', 'NN'), ('+hood', 'N_HOOD')]]
[[('build', 'VB'), ('+ing', 'N_ING')]]
[[('wit', 'NN'), ('+y', 'J_Y'), ('+icism', 'N_ISM')], [('baptize', 'VB'), ('+ism', 'N_ISM')], [('capital', 'NN'), ('+ize', 'V_IZE'), ('+ism', 'N_ISM')], [('bimetal', 'NN'), ('+ism', 'N_ISM')]]
[[('capital', 'JJ'), ('+ist', 'N_IST')], [('machine', 'NN'), ('+ist', 'N_IST')], [('panel', 'NN'), ('+ist', 'N_IST')], [('environ', 'VB'), ('+ment', 'N_MENT'), ('+al', 'J_AL'), ('+ist', 'N_IST')]]
[[('capable', 'JJ'), ('+ility', 'N_ITY')], [('vary', 'VB'), ('+ious', 'J_OUS'), ('+ety', 'N_ITY')], [('norm', 'NN'), ('+al', 'J_AL'), ('+ity', 'N_ITY')], [('loyal', 'JJ'), ('+ty', 'N_ITY')]]
[[('chair', 'VB'), ('+man', 'N_MAN')], [('chair', 'VB'), ('+woman', 'N_MAN')], [('chair', 'VB'), ('+person', 'N_MAN')]]
[[('develop', 'VB'), ('+ment', 'N_MENT')], [('abridge', 'VB'), ('+ment', 'N_MENT')]]
[[('happy', 'JJ'), ('+iness', 'N_NESS')], [('kind', 'JJ'), ('+ness', 'N_NESS')], [('thin', 'JJ'), ('+ness', 'N_NESS')]]
[[('friend', 'NN'), ('+ship', 'N_SHIP')]]
[[('diagnose', 'VB'), ('+sis', 'N_SIS')], [('analyze', 'VB'), ('+sis', 'N_SIS')]]
[[('verify', 'VB'), ('+ication', 'N_TION')], [('admire', 'VB'), ('+ation', 'N_TION')], [('suspect', 'VB'), ('+icion', 'N_TION')], [('add', 'VB'), ('+ition', 'N_TION')], [('decide', 'VB'), ('+sion', 'N_TION')]]
```

Adjective derivations:
```python
tp = []

# adjective: 'able'
tp.append((['readable', 'irritable', 'flammable', 'visible'], ['JJ', 'JJ', 'JJ', 'JJ']))

# adjective: 'al'
tp.append((['influential', 'accidental', 'universal', 'focal', 'economical'], ['JJ', 'JJ', 'JJ', 'JJ', 'JJ']))

# adjective: 'ant'
tp.append((['applicant', 'relaxant', 'pleasant', 'dominant', 'absorbent'], ['JJ', 'JJ', 'JJ', 'JJ', 'JJ']))

# adjective: 'ary'
tp.append((['cautionary', 'imaginary', 'pupillary', 'monetary'], ['JJ', 'JJ', 'JJ', 'JJ']))

# adjective: 'ed'
tp.append((['diffused', 'shrunk'], ['JJ', 'JJ']))

# adjective: 'ful'
tp.append((['beautiful', 'thoughtful', 'helpful'], ['JJ', 'JJ', 'JJ']))

# adjective: 'ic'
tp.append((['fantastic', 'diagnostic', 'analytic', 'poetic'], ['JJ', 'JJ', 'JJ', 'JJ']))

# adjective: 'ing'
tp.append((['dignifying', 'abiding'], ['JJ', 'JJ']))

# adjective: 'ish'
tp.append((['ticklish', 'reddish', 'boyish', 'mulish'], ['JJ', 'JJ', 'JJ', 'JJ']))

# adjective: 'ive'
tp.append((['talkative', 'adjudicative', 'destructive', 'defensive'], ['JJ', 'JJ', 'JJ', 'JJ']))

# adjective: 'less'
tp.append((['countless', 'speechless'], ['JJ', 'JJ']))

# adjective: 'like'
tp.append((['childlike'], ['JJ']))

# adjective: 'ly'
tp.append((['daily', 'weekly'], ['JJ', 'JJ']))

# adjective: 'most'
tp.append((['innermost'], ['JJ']))

# adjective: 'ous'
tp.append((['glorious', 'wondrous', 'marvellous', 'nervous', 'religious'], ['JJ', 'JJ', 'JJ', 'JJ', 'JJ']))

# adjective: 'some'
tp.append((['worrisome', 'troublesome', 'awesome', 'fulsome'], ['JJ', 'JJ', 'JJ', 'JJ']))

# adjective: 'wise'
tp.append((['clockwise', 'likewise'], ['JJ', 'JJ']))

# adjective: 'y'
tp.append((['clayey', 'grouchy', 'runny', 'rumbly'], ['JJ', 'JJ', 'JJ', 'JJ']))

for tokens, postags in tp:
    print(analyzer.decode(tokens, postags, derivation=True, baseonly=False))
```

Output:
```python
[[('read', 'VB'), ('+able', 'J_ABLE')], [('irritate', 'VB'), ('+able', 'J_ABLE')], [('flam', 'VB'), ('+able', 'J_ABLE')], [('vision', 'NN'), ('+ible', 'J_ABLE')]]
[[('influence', 'NN'), ('+tial', 'J_AL')], [('accident', 'NN'), ('+al', 'J_AL')], [('universe', 'NN'), ('+al', 'J_AL')], [('focus', 'NN'), ('+al', 'J_AL')], [('economy', 'NN'), ('+ic', 'J_IC'), ('+al', 'J_AL')]]
[[('apply', 'VB'), ('+icant', 'J_ANT')], [('relax', 'VB'), ('+ant', 'J_ANT')], [('please', 'VB'), ('+ant', 'J_ANT')], [('dominate', 'VB'), ('+ant', 'J_ANT')], [('absorb', 'VB'), ('+ent', 'J_ANT')]]
[[('caution', 'VB'), ('+ary', 'J_ARY')], [('imagine', 'VB'), ('+ary', 'J_ARY')], [('pupil', 'NN'), ('+ary', 'J_ARY')], [('money', 'NN'), ('+tary', 'J_ARY')]]
[[('diffuse', 'VB'), ('+d', 'J_ED')], [('shrink', 'VB'), ('+u+', 'J_ED')]]
[[('beauty', 'NN'), ('+iful', 'J_FUL')], [('thought', 'NN'), ('+ful', 'J_FUL')], [('help', 'VB'), ('+ful', 'J_FUL')]]
[[('fantasy', 'NN'), ('+tic', 'J_IC')], [('diagnose', 'VB'), ('+sis', 'N_SIS'), ('+tic', 'J_IC')], [('analyze', 'VB'), ('+sis', 'N_SIS'), ('+tic', 'J_IC')], [('poet', 'NN'), ('+ic', 'J_IC')]]
[[('dignity', 'NN'), ('+ify', 'V_FY'), ('+ing', 'J_ING')], [('abide', 'VB'), ('+ing', 'J_ING')]]
[[('tickle', 'VB'), ('+ish', 'J_ISH')], [('red', 'VB'), ('+ish', 'J_ISH')], [('boy', 'NN'), ('+ish', 'J_ISH')], [('mule', 'NN'), ('+ish', 'J_ISH')]]
[[('talk', 'VB'), ('+ative', 'J_IVE')], [('adjudicate', 'VB'), ('+ative', 'J_IVE')], [('destruct', 'VB'), ('+ive', 'J_IVE')], [('defense', 'NN'), ('+ive', 'J_IVE')]]
[[('count', 'VB'), ('+less', 'J_LESS')], [('speech', 'NN'), ('+less', 'J_LESS')]]
[[('child', 'NN'), ('+like', 'J_LIKE')]]
[[('day', 'NN'), ('+ily', 'J_LY')], [('week', 'NN'), ('+ly', 'J_LY')]]
[[('inner', 'JJ'), ('+most', 'J_MOST')]]
[[('glory', 'VB'), ('+ious', 'J_OUS')], [('wonder', 'NN'), ('+rous', 'J_OUS')], [('marvel', 'VB'), ('+ous', 'J_OUS')], [('nerve', 'VB'), ('+ous', 'J_OUS')], [('religion', 'NN'), ('+ous', 'J_OUS')]]
[[('worry', 'NN'), ('+isome', 'J_SOME')], [('trouble', 'NN'), ('+some', 'J_SOME')], [('awe', 'NN'), ('+some', 'J_SOME')], [('full', 'JJ'), ('+some', 'J_SOME')]]
[[('clock', 'NN'), ('+wise', 'J_WISE')], [('like', 'JJ'), ('+wise', 'J_WISE')]]
[[('clay', 'NN'), ('+ey', 'J_Y')], [('grouch', 'VB'), ('+y', 'J_Y')], [('run', 'VB'), ('+y', 'J_Y')], [('rumble', 'VB'), ('+y', 'J_Y')]]
```

Adverb derivations:
```python
tp = []

# adverb: 'ly'
tp.append((['electronically', 'easily', 'sadly', 'incredibly'], ['RB', 'RB', 'RB', 'RB']))

for tokens, postags in tp:
    print(analyzer.decode(tokens, postags, derivation=True, baseonly=False))
```

Output:
```python
[
    [('electron', 'NN'), ('+ic', 'J_IC'), ('+ally', 'R_LY')], 
    [('ease', 'VB'), ('+y', 'J_Y'), ('+ily', 'R_LY')], 
    [('sad', 'JJ'), ('+ly', 'R_LY')], 
    [('incredible', 'JJ'), ('+ly', 'R_LY')]
]
```

Finally, if you set `prefix=1` (find longest prefixes) or `prefix=2` (find shortest prefixes), it also analyzes the prefixes (experimental):
```python
tp = []

tp.append((['belittle', 'co-founder', 'super-overlook', 'deuteragonist'], ['VB', 'NN', 'VB', 'NN']))

for tokens, postags in tp:
    print(analyzer.decode(tokens, postags, derivation=True, prefix=2, baseonly=False))
```

Output:
```python
[
    [('little', 'JJ'), ('be+', 'P')], 
    [('found', 'VB'), ('co+', 'P'), ('+er', 'N_ER')], 
    [('look', 'VB'), ('super+', 'P'), ('over+', 'P')], 
    [('agonist', 'NN'), ('deuter+', 'P')]
]
```

## Lemma Tags

| Tag  | Description   | Tag  | Description  |
|------|---------------|------|--------------|
| `CC` | Conjunction   | `MD` | Modal        |
| `CD` | Cardinal      | `NN` | Noun         |
| `DT` | Determiner    | `PR` | Pronoun      |
| `EX` | Existential   | `RB` | Adverb       |
| `FW` | Foreign word  | `UH` | Interjection |
| `GW` | Goes With     | `VB` | Verb         |
| `IN` | Case          | `PU` | Punctuation  |
| `JJ` | Adjective     | `XX` | Unknown      |
| `LS` | List marker   |      |              |

## Inflection Tags

| Tag  | Description                   | Tag  | Description |
|------|-------------------------------|------|-------------|
| `I_3PS` | 3rd-person, singular, present | `I_PLR` | Plural      |
| `I_GRD` | Gerund                        | `I_COM` | Comparative |
| `I_PST` | Past                          | `I_SUP` | Superlative |

## Derivation Tags

* Verb: `V_EN`, `V_FY`, `V_IZE`
* Noun: `N_AGE`, `N_AL`, `N_ANCE`, `N_ANT`, `N_DOM`, `N_EE`, `N_ER`, `N_HOOD`, `N_ING`, `N_ISM`, `N_IST`, `N_ITY`, `N_MAN`, `N_MENT`, `N_NESS`, `N_SHIP`, `N_SIS`, `N_TION`, `N_WARE`
* Adjective: `J_ABLE`, `J_AL`, `J_ANT`, `J_ARY`, `J_ED`, `J_FUL`, `J_IC`, `J_ING`, `J_ISH`, `J_IVE`, `J_LESS`, `J_LIKE`, `J_LY`, `J_MOST`, `J_OUS`, `J_SOME`, `J_WISE`, `J_Y`
* Adverb: `R_LY`
