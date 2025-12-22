# 🟢 Correlation

* <mark style="color:purple;background-color:purple;">**Pearson correlation tells how strongly two values move together in a straight-line pattern**</mark>
* **Simple intuition**
  * Check how much each value differs from its average
  * See whether both go up or down together
  * Scale the result so it always stays between **-1 and +1**
* **Formula**
  * r = Σ\[(xᵢ − x̄)(yᵢ − ȳ)] / √\[Σ(xᵢ − x̄)² · Σ(yᵢ − ȳ)²]
* **Meaning of symbols**
  * xᵢ, yᵢ → individual data points
  * x̄, ȳ → average (mean) of X and Y
  * Σ → sum over all data points
* **How to read the result**
  * <mark style="color:purple;background-color:purple;">**r = +1 → move together perfectly**</mark>
  * <mark style="color:purple;background-color:purple;">**r = −1 → move opposite perfectly**</mark>
  * <mark style="color:purple;background-color:purple;">**r ≈ 0 → no straight-line relationship**</mark>
* **Key takeaway**
  * Measures **linear relationship only**
  * Does **not** mean one causes the other
