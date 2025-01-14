# ChunkDot

Multi-threaded matrix multiplication and cosine similarity calculations. Appropriate for calculating the K most similar items for a large number of items by chunking the item matrix representation (embeddings) and using Numba to accelerate the calculations.

## Usage

```bash
pip install -U chunkdot
```

### Dense embeddings

Calculate the 50 most similar and dissimilar items for 100K items.

```python
import numpy as np
from chunkdot import cosine_similarity_top_k

embeddings = np.random.randn(100000, 256)
# using all you system's memory
cosine_similarity_top_k(embeddings, top_k=50)
# most dissimilar items using 20GB
cosine_similarity_top_k(embeddings, top_k=-50, max_memory=20E9)
```
```
<100000x100000 sparse matrix of type '<class 'numpy.float64'>'
 with 5000000 stored elements in Compressed Sparse Row format>
```

Execution time
```python
from timeit import timeit
import numpy as np
from chunkdot import cosine_similarity_top_k

embeddings = np.random.randn(100000, 256)
timeit(lambda: cosine_similarity_top_k(embeddings, top_k=50, max_memory=20E9), number=1)
```
```
58.611996899999994
```

### Sparse embeddings

Calculate the 50 most similar and dissimilar items for 100K items. Items represented by 10K dimensional vectors and an embeddings matrix of 0.005 density.

```python
from scipy import sparse
from chunkdot import cosine_similarity_top_k

embeddings = sparse.rand(100000, 10000, density=0.005)
# using all you system's memory
cosine_similarity_top_k(embeddings, top_k=50)
# most dissimilar items using 20GB
cosine_similarity_top_k(embeddings, top_k=-50, max_memory=20E9)
```
```
<100000x100000 sparse matrix of type '<class 'numpy.float64'>'
 with 5000000 stored elements in Compressed Sparse Row format>
```

Execution time

```python
from timeit import timeit
from scipy import sparse
from chunkdot import cosine_similarity_top_k

embeddings = sparse.rand(100000, 10000, density=0.005)
timeit(lambda: cosine_similarity_top_k(embeddings, top_k=50, max_memory=20E9), number=1)
```
```
51.87472256699999
```
