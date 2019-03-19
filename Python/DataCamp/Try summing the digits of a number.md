Method 1:
```python
# main thought: change a number to string, then iterate over it, last do addition on int().

def digit_sum(n):
  n=str(n)
  total=0
  for i in n:
    total +=int(i)
  return total
  print total
``` 
Method 2:
```python
# main thought: modulo number by 10 to 10**n, unless result equal with 0 then break, 
# get the modulo result and do addition, 
# and then minus result mutiply the relevant 10** n from the number which get from last iterate.

def digit_sum(n):
  total=0
  while m>=0:
    if n%(10**m)!=0:
      total +=n%(10**m)
      n -=(n%(10**m))*10**m
    else:
      break
  return total
  print total
```
