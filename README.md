一维区间修改+查询的前缀和公式：
$$\sum_{i=1}^{x} d_i = (x+1) \cdot \sum d_i - \sum (i \cdot d_i)$$

二维矩形修改+查询的前缀和公式：
$$\sum_{(i,j)=(1,1)}^{(x,y)} d_{ij} = (x+1) \cdot (y+1) \cdot \sum_{i=1}^{x}\sum_{j=1}^{y} d_{ij} - (y+1) \cdot \sum_{i=1}^{x}\sum_{j=1}^{y} (i \cdot d_{ij}) - (x+1) \cdot \sum_{i=1}^{x}\sum_{j=1}^{y} (j \cdot d_{ij}) + \sum_{i=1}^{x}\sum_{j=1}^{y} (i \cdot j \cdot d_{ij})$$