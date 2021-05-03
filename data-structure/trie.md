# Trie

as known as prefix tree.

### Implementation

```cpp
struct TrieNode {
    TrieNode *next[26] = {};
    int count = 0;
};

class Trie {
private:
    TrieNode root;
    TrieNode* searchToNode(string &prefix) {
        auto node = &root;
        for (char c : prefix) {
            if (!node->next[c - 'a']) return NULL;
            node = node->next[c - 'a'];
        }
        return node;
    }
public:
    void insert(string word) {
        auto node = &root;
        for (char c : word) {
            if (!node->next[c - 'a']) node->next[c - 'a'] = new TrieNode();
            node = node->next[c - 'a'];
        }
        node->count++;
    }

    bool search(string word) {
        auto node = searchToNode(word);
        return node && node->count;
    }

    bool startsWith(string prefix) {
        return searchToNode(prefix);
    }
};
```

## Problems

* [212. Word Search II \(Hard\)](https://leetcode.com/problems/word-search-ii/)
* [1032. Stream of Characters (Hard)](https://leetcode.com/problems/stream-of-characters/)
* [839. Similar String Groups (Hard)](https://leetcode.com/problems/similar-string-groups/)
* [745. Prefix and Suffix Search (Hard)](https://leetcode.com/problems/prefix-and-suffix-search/)