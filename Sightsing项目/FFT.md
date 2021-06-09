## FFT

1. recursive FFT（递归）

   ```
   RECURSIVE-FFT(a)
   	n = a.length
   	if n == 1
   		return a
   	wn = exp(2 * pi * i / n)
   	w = 1
   	arr[0] = (a0, a2, ..., an-2)
   	arr[1] = (a1, a3, ..., an-1)
   	y[0] = RECURSIVE-FFT(a[0])
   	y[1] = RECURSIVE-FFT(a[1])
   	for k=0 to n/2 - 1
   		yk = y[0][k] + wy[1][k]
   		yk+n/2 = y[0][k] - wy[1][k]
   		w = w * wn
   	return y
   ```

2. 迭代 FFT