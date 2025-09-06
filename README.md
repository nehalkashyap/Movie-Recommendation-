import pandas as pd
import math

movies = pd.read_csv("tmdb_5000_movies.csv")
movies = movies [["title","genres","keywords","overview","tagline"]]
movies = movies.fillna('')
movies["tags"] = movies["genres"] + " " + movies["keywords"] + " " + movies["overview"] + " " + movies["tagline"]

def text_to_vector(text):
    words = text.lower().split()
    freq = {}
    for word in words:
        freq[word] = freq.get(word, 0) + 1
    return freq

def cosine_similarity(v1, v2):
common_words = set(v1.keys()) & set(v2.keys())
    numerator = sum(v1[word] * v2[word] for word in common_words)
    sum1 = sum(v1[word]**2 for word in v1.keys())
    sum2 = sum(v2[word]**2 for word in v2.keys())
    denominator = math.sqrt(sum1) * math.sqrt(sum2)
    if denominator == 0:
        return 0
return numerator / denominator

vectors = [text_to_vector(t) for t in movies["tags"]]
    
def recommend(movie_name):
    if movie_name not in movies["title"].values:
        print("Movie not found")
        return
    index = movies[movies["title"] == movie_name].index[0]
    similarities = []
    for i in range(len(vectors):
score = cosine_similarity(vectors[index], vectors[i])
        similarities.append((i, score))
    similarities = sorted(similarities, key=lambda x: x[1], reverse=True)
    print(f"Top 5 recommendations for '{movie_name}':")
    for i in similarities[1:6]:
        print(movies.iloc[i[0]].title)

recommend("Avatar")
recommend("The Dark Knight Rises")
recommend("Pirates of the Caribbean: At World's End")
