# 🟢 Operator

* <mark style="color:purple;background-color:purple;">**`operator`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**is a built-in Python module that provides functions corresponding to Python operators.**</mark>
* <mark style="color:purple;background-color:purple;">**Allows you to use operators like**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`+`**</mark><mark style="color:purple;background-color:purple;">**,**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`-`**</mark><mark style="color:purple;background-color:purple;">**,**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`*`**</mark><mark style="color:purple;background-color:purple;">**,**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`<`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**as functions.**</mark>
* <mark style="color:purple;background-color:purple;">**Useful when you want to pass an operation as a function to other functions like**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`map`**</mark><mark style="color:purple;background-color:purple;">**,**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`reduce`**</mark><mark style="color:purple;background-color:purple;">**, or frameworks that merge data.**</mark>
* Common functions:
  * `operator.add(x, y)` → `x + y`
  * `operator.sub(x, y)` → `x - y`
  * `operator.mul(x, y)` → `x * y`
  * `operator.truediv(x, y)` → `x / y`
  * `operator.lt(x, y)` → `x < y`
  * `operator.eq(x, y)` → `x == y`
* Example with list merging:

```python
import operator

list1 = [1, 2]
list2 = [3, 4]

merged_list = operator.add(list1, list2)
print(merged_list)  # [1, 2, 3, 4]
```
