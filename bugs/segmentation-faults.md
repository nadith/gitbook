---
description: Various common seg fault issues
---

# Segmentation Faults

## When accessing a 2d array via a "pointer to a 2d array" using bracket notation, it crashed. Why?

> Please have a look at the following code:
>
> ```c
> *arr2dPtr[10][20] /* crashed */
> *(*(*arr2dPtr + 10) + 20) /* did not crash */
> ```

Remember operator precedence (\[ ] comes before \*) is what matters here. If arr2dPtr needs to be dereferenced before using the bracket notation, you can fix the program as below:

```c
(*arr2dPtr)[10][20] /* first dereference arr2dPtr, then use [10][20] */
```

