## Typing Hint in Python
---
#### Sự khác nhau giữa Union và Optional
1. **Optional**:
    
    - `Optional` là một type hint trong Python, được định nghĩa trong module `typing`.
    - `Optional[T]` là một shorthand cho `Union[T, None]`, nghĩa là một giá trị có thể là `T` hoặc `None`.
    - Nó được sử dụng khi một biến hoặc tham số có thể là `None` (hoặc không có giá trị) hoặc một giá trị khác của type `T`.
    - Ví dụ: `Optional[int]` có nghĩa là một giá trị có thể là `int` hoặc `None`.
2. **Union**:
    
    - `Union` là một type hint trong Python, được định nghĩa trong module `typing`.
    - `Union[T1, T2, ..., Tn]` là một type hint cho một giá trị có thể là `T1`, `T2`, ..., hoặc `Tn`.
    - Nó được sử dụng khi một biến hoặc tham số có thể có nhiều kiểu dữ liệu khác nhau.
    - Ví dụ: `Union[int, float, str]` có nghĩa là một giá trị có thể là `int`, `float`, hoặc `str`.