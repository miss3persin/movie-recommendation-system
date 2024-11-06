# Movie Recommendation System

This project implements a **Movie Recommendation System** using the **Cosine Similarity** algorithm. The system recommends movies based on their similarity to a user's favorite movie, using features such as **genres**, **keywords**, **tagline**, **cast**, and **director**.

---

## Overview

The recommendation system works by finding movies similar to the user’s input movie title. It uses **Cosine Similarity** to measure similarity between movies based on selected features and suggests movies that closely match the given title.

---

## Dataset Description

The dataset includes various details about movies, such as budget, genres, production companies, etc. However, only the following features were used for similarity calculations:

- **genres**: The genres of the movie (e.g., Action, Comedy)
- **keywords**: Keywords associated with the movie
- **tagline**: The tagline or tagline phrase for the movie
- **cast**: Main actors/actresses in the movie
- **director**: Director of the movie

### Example of Dataset Structure:
| Index | Title           | Genres         | Keywords         | Tagline              | Cast                   | Director      |
|-------|------------------|----------------|------------------|----------------------|------------------------|---------------|
| 0     | The Matrix       | Action, Sci-Fi | virtual reality  | "Free your mind"     | Keanu Reeves, ...      | Wachowskis    |
| 1     | Inception       | Action, Thriller | dream, subconscious | "Your mind is the scene of the crime" | Leonardo DiCaprio, ... | Christopher Nolan |

---

## Preprocessing Steps

1. **Data Cleaning**: Missing values were replaced with appropriate placeholders for effective processing.
2. **Feature Vectorization**: The selected features were converted to a vectorized format suitable for computing similarity.
3. **Cosine Similarity**: Calculated similarity scores between movies to find close matches.

---

## Model Usage

To use this model, input the name of a movie to receive a list of similar movies. The model will suggest movies based on their cosine similarity score to the input movie.

### Example Code:

```python
from difflib import get_close_matches

def suggest_similar_movies(movie_name, df, similarity, num_suggestions=20):
    # Get the list of all titles in the dataset
    list_of_all_titles = df['title'].tolist()
    # Find the closest match for the input title
    find_close_match = get_close_matches(movie_name, list_of_all_titles)
    if not find_close_match:
        return "No close match found for the movie."

    close_match = find_close_match[0]
    index_of_the_movie = df[df.title == close_match]['index'].values[0]
    similarity_score = list(enumerate(similarity[index_of_the_movie]))
    sorted_similar_movies = sorted(similarity_score, key=lambda x: x[1], reverse=True)

    # Print out the movie suggestions
    print('Movies suggested for you: \n')
    for i, movie in enumerate(sorted_similar_movies[1:num_suggestions+1], start=1):  # skip the first match itself
        index = movie[0]
        title_from_index = df[df.index == index]['title'].values[0]
        print(f'{i}. {title_from_index}')

# User input
movie_name = input('Enter your favorite movie: ')
print('')
print('')
suggest_similar_movies(movie_name, df, similarity)
```
