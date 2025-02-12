# DSA-project
text autocomplete system  

#pseudocode:
Algorithm: Text Autocomplete System

1. Preprocess Dataset:
   - Load dataset from file.
   - Tokenize into words.
   - Clean words (lowercase, remove punctuation).
   - Store cleaned words in a list.

2. Build Trie:
   - Initialize Trie with root node.
   - For each word in the dataset:
     - Insert word into Trie.

3. Autocomplete:
   - While True:
     - Prompt user for prefix.
     - If prefix == "exit":
       - Break loop.
     - Else:
       - Search Trie for prefix.
       - Display suggestions.

4. Search Trie:
   - Start at root node.
   - For each character in prefix:
     - If character not in current node's children:
       - Return empty list.
     - Move to child node.
   - Use helper function to collect all words starting from current node.

5. Helper Function (Find All Words):
   - If current node marks end of word:
     - Add word to result list.
   - For each child node:
     - Recursively find all words starting from child node.
   - Return result list
   - 

#source code:
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return []
            node = node.children[char]
        return self._get_all_words(node, prefix)

    def _get_all_words(self, node, prefix):
        words = []
        if node.is_end_of_word:
            words.append(prefix)
        for char, child_node in node.children.items():
            words.extend(self._get_all_words(child_node, prefix + char))
        return words

# Example Usage
trie = Trie()
words = ["shruti", "shristi", "sharmila", "sheela", "shriniwas"]
for word in words:
    trie.insert(word)

prefix = "sh"
suggestions = trie.search(prefix)
print(f"Suggestions for '{prefix}': {suggestions}")
