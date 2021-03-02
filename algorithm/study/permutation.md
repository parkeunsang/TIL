# 순열

```Python
arr = [1, 2, 3]
N = 3
sel = [0] * N
check = [0] * N

def perm(idx):
    if idx == N:
        print(sel)
    else:
        for i in range(N):
            if check[i] == 0:
                sel[idx] = arr[i]
                check[i] = 1
                perm(idx+1)
                check[i] = 0
perm(0)
```

```Python
def perm(nums, idx):
    if idx == N:
        print(nums)
        return
   	
    for i in range(idx, N):
        nums[i], nums[idx] = nums[idx], nums[i]
        perm(nums, idx+1)
        nums[i], nums[idx] = nums[idx], nums[i]
        
nums = [1,2,3,4]
perm(nums, 1)
```

