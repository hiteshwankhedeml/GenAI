# 🟢 Annotated

* `Annotated` is from Python’s `typing` module.
* <mark style="color:purple;background-color:purple;">**Lets you attach metadata to a type without changing the type itself.**</mark>
* Syntax: `Annotated[Type, metadata]`
* Example: `Annotated[list[str], operator.add]` → type is `list[str]`, metadata is `operator.add`.
* Metadata is **ignored by plain Python** at runtime.
* Useful for **frameworks or tools** that read metadata to decide how to handle the type.
* <mark style="color:purple;background-color:purple;">**In agent frameworks, tells how to merge multiple states automatically.**</mark>
* <mark style="color:purple;background-color:purple;">**Without**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`Annotated`**</mark><mark style="color:purple;background-color:purple;">**, Python works fine manually, but frameworks won’t know how to merge fields automatically.**</mark>
* Common usage: `operator.add` for lists, `operator.or_` for bools, `operator.add` for numbers, etc.
