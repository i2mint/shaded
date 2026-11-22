# shaded.chained_spectral_projector

Chained spectral projectors

### Functions

| [`keep_only_indices`](#shaded.chained_spectral_projector.keep_only_indices)(indices[, n_freq])              | Makes a matrix which when a spectra is multiplied with it, only the entires in the list indices are kept   |
|----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| [`learn_chain_proj_matrix`](#shaded.chained_spectral_projector.learn_chain_proj_matrix)(X[, y, chain, ...])       | A function successively learning a projections matrix on the residue of the previous one.                  |
| [`learn_spect_proj`](#shaded.chained_spectral_projector.learn_spect_proj)(X[, y, spectral_proj_name, ...]) | Function to learn each of the important spectral projection                                                |

### Classes

| [`GeneralProjectionLearner`](#shaded.chained_spectral_projector.GeneralProjectionLearner)([chain, indices, ...])   |    |
|----------------------------------------------------------------------------------------------------|----|

### *class* shaded.chained_spectral_projector.GeneralProjectionLearner(chain=({'args': {'n_components': 5}, 'type': 'pca'},), indices=None, n_freq=1025)

Bases: `BaseEstimator`, `TransformerMixin`

#### fit(X, y=None, ortho=True)

Ortho set to True will make it so that the projection into the subspace will be “direction agnostic”, which
makes sense in terms of pure dimension reduction. But for some model, like lda, the length of the vectors onto
which we will project represent the relative importance of those directions. In that case it may make sense to
set ortho to False

* **Parameters:**
  * **X** – array of vectors
  * **y** – array of tags
  * **ortho** – boolean, whether or not to orthonormalize the vector onto which to project
* **Returns:**

#### set_fit_request(, ortho='$UNCHANGED$')

Configure whether metadata should be requested to be passed to the `fit` method.

Note that this method is only relevant when this estimator is used as a
sub-estimator within a meta-estimator and metadata routing is enabled
with `enable_metadata_routing=True` (see `sklearn.set_config()`).
Please check the User Guide on how the routing
mechanism works.

The options for each parameter are:

- `True`: metadata is requested, and passed to `fit` if provided. The request is ignored if metadata is not provided.
- `False`: metadata is not requested and the meta-estimator will not pass it to `fit`.
- `None`: metadata is not requested, and the meta-estimator will raise an error if the user provides it.
- `str`: metadata should be passed to the meta-estimator with this given alias instead of the original name.

The default (`sklearn.utils.metadata_routing.UNCHANGED`) retains the
existing request. This allows you to change the request for some
parameters and not others.

#### Versionadded
Added in version 1.3.

* **Parameters:**
  **ortho** ([`Union`](https://docs.python.org/3/library/typing.html#typing.Union)[[`bool`](https://docs.python.org/3/builtins/functions.html#bool), [`None`](https://docs.python.org/3/builtins/constants.html#None), [`str`](https://docs.python.org/3/builtins/stdtypes.html#str)]) – Metadata routing for `ortho` parameter in `fit`.
* **Returns:**
  **self** – The updated object.
* **Return type:**
  [`GeneralProjectionLearner`](#shaded.chained_spectral_projector.GeneralProjectionLearner)

### shaded.chained_spectral_projector.keep_only_indices(indices, n_freq=1025)

Makes a matrix which when a spectra is multiplied with it, only the entires in the list indices are kept

* **Parameters:**
  * **indices** – a list of indices to keep
  * **n_freq** – the total number of frequencies in the spectra
* **Returns:**
  a matrix of size (n_freq, len(indices))

### shaded.chained_spectral_projector.learn_chain_proj_matrix(X, y=None, chain=({'args': {'n_components': 5}, 'type': 'pca'},), indices=None, n_freq=1025)

A function successively learning a projections matrix on the residue of the previous one. The projections
matrices are then concatenated and return as one single projection matrix. Note that the final projection
matrix may not produce fvs of the size the sum of the components of each part, i.e., care must be taken
to ensure each classes called must be able to produce the required number of components. For example,
if the number of classes is 10, then lda can only produce 9 dimensions. To obtain say 12 dimension, the user
will need to chain two lda’s, for example with size 9 and 3 respectively.

* **Parameters:**
  * **X** – the fvs, an array of size n\*k
  * **y** – the classes, an array of size n
  * **chain** – a tuple of dictionaries each containing the type of projection along with its parameters
  * **indices** – the indices of the spectra to work with, anything else is discarded
  * **n_freq** – the total number of entries from the spectra. Only needed if n_freq is not None, in order to
    determine the size of the freq_selection_matrix
* **Returns:**
  a single projection matrix

### shaded.chained_spectral_projector.learn_spect_proj(X, y=None, spectral_proj_name='pca', clustering_meth='KMeans', clustering_options=('KMeans', 'SpectralClustering', 'AffinityPropagation', 'AgglomerativeClustering', 'Birch', 'MeanShift'), kwargs_feat=None, kwargs_clust=None)

Function to learn each of the important spectral projection

* **Parameters:**
  * **X** – the fvs, an array of size n\*k
  * **y** – the classes, an array of size n
  * **spectral_proj_name** – a string of the name of the featurizer
  * **args** – extra argument to be passed to the featurizer class
* **Returns:**
  a matrix in the form of a numpy array
