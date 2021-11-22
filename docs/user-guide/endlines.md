# End lines

The `endlines` pipeline uses a model to classify each end line as a space or as a real end line.

The `endlinesmodel` is an unsupervised algorithm based on the work of {footcite:t}`zweigenbaum2016`.

## Declared extensions

The `endlines` pipeline declares one [Spacy extensions](https://spacy.io/usage/processing-pipelines#custom-components-attributes), on both `Span` and `Token` objects. The `end_line` attribute is a boolean, set to `True` if the pipeline predicts that the new line is an end line character. Otherwhise, it is  set to `False` if the new line is classified as a space.

## Usage

The following example shows a simple usage.

### Train

```python
import spacy
from edsnlp.pipelines.endlines.endlinesmodel import EndLinesModel
import pandas as pd
from spacy import displacy

nlp = spacy.blank("fr")

texts = [
    """Le patient est arrivé hier soir.
Il est accompagné par son fils

ANTECEDENTS
Il a fait une TS en 2010;
Fumeur, il est arreté il a 5 mois
Chirurgie de coeur en 2011
CONCLUSION
Il doit prendre
le medicament indiqué 3 fois par jour. Revoir médecin
dans 1 mois.
DIAGNOSTIC :

Antecedents Familiaux:
- 1. Père avec diabete

""",
    """J'aime le \nfromage...\n""",
]

docs = list(nlp.pipe(texts))

# Train and predict an EndLinesModel
endlines = EndLinesModel(nlp=nlp)

df = endlines.fit_and_predict(docs)
df.head()

PATH = "path_to_save"
endlines.save(PATH)
```

### Predict

```python
import edsnlp.components

nlp = spacy.blank("fr")
PATH = "path_to_save"
nlp.add_pipe("endlines", config=dict(model_path=PATH))

docs = list(nlp.pipe(texts))

doc_exemple = docs[1]

doc_exemple

doc_exemple.ents = tuple(
    s for s in doc_exemple.spans["new_lines"] if s.label_ == "space"
)

displacy.render(doc_exemple, style="ent", options={"colors": {"space": "red"}})
```

## Performance

The pipeline's performance is still being evaluated.

## Authors and citation

The `endlines` pipeline was developed at the Data and Innovation unit, IT department, AP-HP. Based on the work of {footcite:t}`zweigenbaum2016`.

## References

```{eval-rst}
.. footbibliography::
```