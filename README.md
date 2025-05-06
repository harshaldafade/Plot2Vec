
# 🎬 **Plot2Vec**  
### _A Plot-Based Semantic Movie Recommendation Engine using Sentence Embeddings, YAKE, and FAISS_

---

## 📌 Overview

This project builds an intelligent movie recommendation system that suggests similar movies based on **plot descriptions**, not metadata or ratings. Using **sentence embeddings**, **keyword extraction**, and **semantic similarity search**, it allows users to input a plot or select an existing movie and get contextually similar movies in return.

The system leverages:
- **[Sentence Transformers](https://www.sbert.net/)** for encoding movie plots.
- **[YAKE](https://github.com/LIAAD/yake)** for unsupervised keyword extraction.
- **[FAISS](https://github.com/facebookresearch/faiss)** for fast similarity search over vectorized embeddings.

---

## 🧠 Key Features

- 📚 **Semantic Understanding** of movie plots using sentence-level embeddings.
- 🧩 **Custom Plot Input**: Just paste a plot, and get recommendations!
- 🧠 **Keyword Extraction**: (Optional) Uses YAKE to extract key phrases for alternate embeddings.
- 🧮 **Aggregation of Sentence Embeddings**: Mean or Sum pooling supported.
- ⚡ **Fast Similarity Search** using FAISS for real-time querying over thousands of plots.
- ✅ **Offline Capability**: Once embeddings are generated, no API calls required.

---

## 📂 Dataset

- [Wiki Movie Plots Deduped (Kaggle)](https://www.kaggle.com/datasets/ashirwadsangwan/imdb-dataset) (sampled subset used)
- You can replace it with your own dataset in CSV format with a `Plot` and `Title` column.

---

## 🧰 Installation

```bash
pip install nltk
pip install -q sentence_transformers
pip install faiss-cpu
pip install yake
```

You may also need:

```python
import nltk
nltk.download('punkt')
```

---

## 🚀 How It Works

### 1. **Preprocessing**
- A sample of 2,000 plots is drawn from the dataset.
- Each plot is sentence-tokenized using NLTK.

### 2. **Sentence Embeddings**
- Each sentence is passed to the `all-mpnet-base-v2` Sentence Transformer model.
- Embeddings are aggregated (`sum` or `mean`) to form a plot-level vector.

### 3. **Similarity Computation**
- All plot embeddings are indexed in a FAISS vector store.
- When a user provides a plot or selects a movie, its embedding is compared using L2 distance.
- The top N similar plots are returned.

### 4. **Keyword-based Search (Optional)**
- YAKE is used to extract keywords from each plot.
- Embeddings for these keywords can also be used for comparison.

---

## 🧪 Example Usage

```python
# Generate embeddings
embeddings = get_embeddings(subset_500)

# Save embeddings for later
with open('embeddings.pkl', 'wb') as file:
    pickle.dump(embeddings, file)

# Get similar plots using index
indexes, distances = get_similarity(plot_index=1, embeddings=embeddings, num_of_similars=5)

# Get similar plots using custom user input
get_similarity_from_plot(user_input=my_plot_text, embeddings=embeddings, num_of_similars=4, print_plot=True)
```

### Example Input:
```text
Luke Skywalker lives on a farm on Tatooine... [Star Wars Plot]
```

### Example Output:
```text
I recommend: ['Star Wars', 'The Empire Strikes Back', 'Battlestar Galactica', 'Dune']
```

---

## 📈 Results

Even when tested with creative, quirky, or obscure plots, the system often returns highly relevant and interesting recommendations—many from different genres, proving the **semantic strength** of this approach.

---

## ✅ To-Do

- [ ] Switch between sentence-based and keyword-based embeddings easily.
- [ ] Add a web front-end using Streamlit or Flask.
- [ ] Cache or persist FAISS index for scalability.
- [ ] Support multi-language plot input and recommendation.
- [ ] Implement plot visualization using t-SNE or PCA.

---

## 📜 License

MIT License.  
Feel free to fork and use this system for your own movie recommendation experiments or projects!
