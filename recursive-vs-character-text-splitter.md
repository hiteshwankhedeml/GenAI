# 🟠 Recursive vs Character Text Splitter

* <mark style="color:purple;background-color:purple;">**RecursiveCharacterTextSplitter respects paragraph and sentence boundaries and packs as much as fits into chunk size without breaking sentences.**</mark>
* <mark style="color:purple;background-color:purple;">**CharacterTextSplitter splits strictly by character count using the separator, which can split sentences across chunks.**</mark>

| Paragraph   | Length (chars) | Notes                         |
| ----------- | -------------- | ----------------------------- |
| Paragraph 1 | 800            | Single paragraph              |
| Paragraph 2 | 100            | Single paragraph              |
| Paragraph 3 | 200            | Two sentences: 50 + 150 chars |
| **Total**   | **1100**       | Combined length               |

* **Chunk size** = 1000 characters
* **Separator** = `" "` (space) for CharacterTextSplitter

***

#### 🔹 **Splitter Behavior Comparison**

| Feature                      | RecursiveCharacterTextSplitter                                                                                         | CharacterTextSplitter                                                                                           |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Splitting strategy           | <mark style="color:purple;background-color:purple;">**Preserves structure: Paragraph → Sentence → Word → Char**</mark> | <mark style="color:purple;background-color:purple;">**Fixed-length chunks, splits at nearest separator**</mark> |
| Paragraph-aware              | ✅ Yes                                                                                                                  | ❌ No                                                                                                            |
| Sentence-aware               | ✅ Yes                                                                                                                  | ❌ No                                                                                                            |
| Word-aware                   | ✅ Yes                                                                                                                  | ✅ Yes (due to separator)                                                                                        |
| Avoid mid-sentence splitting | ✅ Yes (unless sentence > chunk size)                                                                                   | ❌ No                                                                                                            |
| Avoid mid-word splitting     | ✅ Yes                                                                                                                  | ✅ Yes                                                                                                           |
| Ideal use case               | Complex documents where context matters                                                                                | Simple/flat text                                                                                                |

***

#### 🔹 **Chunking Outcome Using the Example**

| Chunk # | RecursiveCharacterTextSplitter              | Size (chars) | CharacterTextSplitter                           | Size (chars) |
| ------- | ------------------------------------------- | ------------ | ----------------------------------------------- | ------------ |
| 1       | Paragraph 1 (800) + Paragraph 2 (100)       | 900          | Paragraph 1 + Paragraph 2 + part of Paragraph 3 | \~1000       |
| 2       | Paragraph 3 (Sentence 1 + Sentence 2 = 200) | 200          | Remaining part of Paragraph 3                   | \~100        |

* **Recursive**: Paragraph 3 fits entirely in chunk 2, sentences kept intact.
* **Character**: Cuts Paragraph 3 mid-sentence to keep chunk size \~1000.
