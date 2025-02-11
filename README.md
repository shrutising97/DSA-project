# DSA-project
text autocomplete system  
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
