# DSA-project
text autocomplete system
import nltk
import random
from nltk.util import ngrams
from collections import defaultdict

nltk.download('punkt')

class TextAutocomplete:
    def __init__(self, corpus, n=3):
        self.n = n  # N-gram size
        self.model = defaultdict(list)
        self._train_model(corpus)

    def _train_model(self, corpus):
        tokens = nltk.word_tokenize(corpus.lower())
        n_grams = list(ngrams(tokens, self.n, pad_left=True, pad_right=True))
        
        for gram in n_grams:
            prefix, next_word = tuple(gram[:-1]), gram[-1]
            self.model[prefix].append(next_word)

    def predict(self, text):
        tokens = nltk.word_tokenize(text.lower())
        prefix = tuple(tokens[-(self.n-1):])
        predictions = self.model.get(prefix, [])
        return random.choice(predictions) if predictions else "No suggestion"

# Example Corpus
corpus = "Hello, how are you? I am fine. How about you? I hope you are doing well."
autocomplete = TextAutocomplete(corpus, n=2)

# Predict next word
input_text = "how are"
print(f"Suggested word: {autocomplete.predict(input_text)}")
