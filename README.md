# 🎬 Mean Normalization in Collaborative Filtering

---

## 📌 1. Why We Need Mean Normalization
When building a recommender system (e.g., movie ratings from 0–5 or 1–5 stars):

- **Without mean normalization**, a **new user** with no ratings will get:
  - User parameters `w = [0, 0]` (minimized by regularization)
  - Bias `b = 0` (default initialization)
  - Prediction formula:
    ```
    prediction = w · x + b = 0
    ```
    → Predicts **0 stars for all movies** (unrealistic!).

---

## 📉 2. The Cold-Start Problem
Example:
- New user **Eve** has not rated any movies.
- Model predicts `0` for all movies → This doesn’t reflect real user behavior.
- We want to guess **something reasonable**, like the average rating of each movie.

---

## 🔹 3. What is Mean Normalization?
**Idea:** Shift ratings so that each movie’s ratings have a mean of `0`.

**Steps:**
1. **Arrange ratings in a matrix** `Y`  
   - Rows = Users  
   - Columns = Movies

2. **Compute average rating per movie** (`μ` vector)  
   Example:  
   - Movie 1 ratings: `5, 0, 5, 0` → Average = `2.5`
   - Do this for all movies.

3. **Subtract the mean** from each rating:
   Y[i, j] = Y[i, j] - μ[j]
(Skip missing ratings)

4. **Train collaborative filtering** on these normalized ratings.

5. **Add the mean back when predicting**:  
    prediction = (w_user · x_movie + b_user) + μ[movie]

---

## 🎯 4. Effect on New Users
- If user has **no ratings**:
- `w = [0,0]`, `b = 0`
- Prediction = `μ[movie]`
- Meaning: Guess the **average rating of that movie** → More reasonable.

---

## ✅ 5. Benefits
- Handles **cold-start users** better.
- Makes **predictions more realistic**.
- Speeds up optimization slightly.
- Improves numerical stability.

---

## 🔄 6. Row vs Column Normalization
| Normalization Type  | Good For         | Notes |
|---------------------|------------------|-------|
| **Row (movies)**    | New users        | Recommended |
| **Column (users)**  | New movies       | Less useful; collect some ratings first |

---

## 📜 7. Formula Recap
**Normalization:**
Y[i, j] = Y[i, j] - μ[j]

**Prediction:**
y_hat[i, j] = (w_user · x_movie + b_user) + μ[movie]
