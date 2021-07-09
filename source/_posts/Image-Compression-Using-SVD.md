---
title: Image Compression Using SVD
tags: [ML]
categories:
  - [ML]
date: 2021-07-10 04:08:29
description: 利用SVD奇异值分解压缩图像
cover: bgimg.jpg
top_img: false
katex: true
toc_number: false
---

```python
import numpy as np
import matplotlib.pyplot as plt
```

```python
def svdCompression(image, k):
    compress_image = np.zeros(image.shape)
    for i in range(3):
        U, sigma, VT = np.linalg.svd(image[:,:,i])  # rgb -> svd
        print("sigma size: ", sigma.size)
        Msigma = np.zeros( (k,k) )
        for snglr in range(k):
            Msigma[snglr,snglr] = sigma[snglr]
        compress_image[:,:,i] = ((U[:, 0:k]).dot(Msigma)).dot(VT[0:k, :])
    # 
    for i in range(3):
        MAX = np.max(compress_image[:, :, i])
        MIN = np.min(compress_image[:, :, i])
        compress_image[:, :, i] = (compress_image[:, :, i] - MIN) / (MAX - MIN)
    compress_image  = np.round(compress_image * 255).astype('int')
    
    # show picture
    plt.figure( figsize=(5,5) )
    plt.imshow(compress_image)
    plt.title("Compression")
    plt.show()
    
```

## 原图 k=600
```python
file = "wechat.jpg"
image = plt.imread(file)
plt.figure( figsize=(5,5) )
plt.imshow(image)
plt.title("Original")
plt.show()
```
![png](output_2_0.png)


## 令K=1（取前1个奇异值）
```python
svdCompression(image, 1)
```

    sigma size:  600
    sigma size:  600
    sigma size:  600
    


![png](output_3_1.png)


## 令K=10（取前10个奇异值）
```python
svdCompression(image, 10)
```

    sigma size:  600
    sigma size:  600
    sigma size:  600
    


![png](output_4_1.png)



```python
svdCompression(image, 20)
```

    sigma size:  600
    sigma size:  600
    sigma size:  600
    


![png](output_5_1.png)


## 令K=50（取前50个奇异值）
```python
svdCompression(image, 50)
```

    sigma size:  600
    sigma size:  600
    sigma size:  600
    


![png](output_6_1.png)



```python
svdCompression(image, 100)
```

    sigma size:  600
    sigma size:  600
    sigma size:  600
    


![png](output_7_1.png)


## 令K=500（取前500个奇异值）
```python
svdCompression(image, 500)
```

    sigma size:  600
    sigma size:  600
    sigma size:  600
    


![png](output_8_1.png)

