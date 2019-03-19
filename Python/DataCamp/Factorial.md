All right! Now we're cooking. Let's try a factorial problem.

To calculate the factorial of a non-negative integer x, just multiply all the integers from 1 through x. For example:
```python
factorial(4) would equal 4 * 3 * 2 * 1, which is 24.
factorial(1) would equal 1.
factorial(3) would equal 3 * 2 * 1, which is 6.

def factorial(x): 
  total=1 
  if x==1: 
    return total 
  else: 
    while x>1:  # Here should pay attention, can't use "for" instead of "while", because "for" need to figure out the range # 
      total *=x 
      x -=1 
    return total 
  return total
```
