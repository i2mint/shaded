# shaded.linalg_utils

Linear algebra utils

### Functions

| `make_proj_matrix`(span_vectors)                                                |                                                                                                                                  |
|---------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| [`null_space`](#shaded.linalg_utils.null_space)(A[, rcond])         | Find an orthonormal matrix whose rows span the nullspace of the matrix A.                                                        |
| [`orth`](#shaded.linalg_utils.orth)(A)                        | Orthonormalize the matrix A, i.e, replace it with another matrix spanning the same subspace but:                                 |
| [`projection`](#shaded.linalg_utils.projection)(basis, vectors)     | The vectors live in a k dimensional space S and the columns of the basis are vectors of the same space spanning a subspace of S. |
| `projection_`(proj_mat, vectors)                                                |                                                                                                                                  |
| [`residue`](#shaded.linalg_utils.residue)(span_vectors, vectors) | find the residue of each of vectors after projection in basis                                                                    |

### shaded.linalg_utils.null_space(A, rcond=None)

Find an orthonormal matrix whose rows span the nullspace of the matrix A. Put another way, find a maximal
number of row vectors, each of which has a 0 dot product with all the rows of A, while those row vectors are
also normal to each other and of unit length

```pycon
>>> A = np.array([[1, 0, 0]])
>>> null_space(A)
array([[0., 1., 0.],
       [0., 0., 1.]])
```

```pycon
>>> A = np.array([[1, -2, 6, 8], [2.1, 6, 20, -3]])
>>> B = null_space(A)
```

The vectors are normal

```pycon
>>> np.sum(B ** 2, axis=1)
array([1., 1.])
```

### shaded.linalg_utils.orth(A)

Orthonormalize the matrix A, i.e, replace it with another matrix spanning the same subspace but:

1. with minimal number of rows
2. rows normal to each other
3. rows of length 1

* **Parameters:**
  **A** – a n-by-k array
* **Returns:**
  a n-by-k array whose row spans the same space as the rows of A and with the normal rows

```pycon
>>> M = np.array([[1,2,3], [3,4,5]])
>>> OM = orth(M)
>>> # checking the product of O and its transpose is the identity
>>> np.allclose(np.dot(OM, OM.T), np.eye(2))
True
```

### shaded.linalg_utils.projection(basis, vectors)

The vectors live in a k dimensional space S and the columns of the basis are vectors of the same
space spanning a subspace of S. Gives a representation of the projection of vector into the space
spanned by basis in term of the basis.

* **Parameters:**
  * **basis** – an n-by-k array, a matrix whose vertical columns are the vectors of the basis
  * **vectors** – an m-by-k array, a vector to be represented in the basis
* **Returns:**
  an m-by-k array

### shaded.linalg_utils.residue(span_vectors, vectors)

find the residue of each of vectors after projection in basis

* **Parameters:**
  * **span_vectors** – an n-by-k array, spanning a vector space A
  * **vectors** – an n-by-l array
* **Returns:**
  an n-by-l array, the residues of vectors with the respect of the projection in the space spanned
  by span_vectors

```pycon
>>> span_vectors = np.array([[1, 0], [0, 1]])
>>> vectors = np.array([[2, 3]])
>>> print(residue(span_vectors, vectors))
[[0 0]]
>>> span_vectors = np.array([[1, 0],[0, 0]])
>>> vectors = np.array([[2, 3]])
>>> print(residue(span_vectors, vectors))
[[0 3]]
```
