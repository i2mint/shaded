# shaded.pair_wise_lda

Pairwise LDA

### Classes

| [`PairwiseLda`](#shaded.pair_wise_lda.PairwiseLda)([n_components])   | A class finding the hyperplane spanning the lines found by LDA when trying to separate the pair of classes   |
|--------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|

### *class* shaded.pair_wise_lda.PairwiseLda(n_components=None)

Bases: `BaseEstimator`, `TransformerMixin`

A class finding the hyperplane spanning the lines found by LDA when trying to
separate the pair of classes

#### set_fit_request(, classes='$UNCHANGED$', normal='$UNCHANGED$')

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
  * **classes** ([`Union`](https://docs.python.org/3/library/typing.html#typing.Union)[[`bool`](https://docs.python.org/3/builtins/functions.html#bool), [`None`](https://docs.python.org/3/builtins/constants.html#None), [`str`](https://docs.python.org/3/builtins/stdtypes.html#str)]) – Metadata routing for `classes` parameter in `fit`.
  * **normal** ([`Union`](https://docs.python.org/3/library/typing.html#typing.Union)[[`bool`](https://docs.python.org/3/builtins/functions.html#bool), [`None`](https://docs.python.org/3/builtins/constants.html#None), [`str`](https://docs.python.org/3/builtins/stdtypes.html#str)]) – Metadata routing for `normal` parameter in `fit`.
* **Returns:**
  **self** – The updated object.
* **Return type:**
  [`PairwiseLda`](#shaded.pair_wise_lda.PairwiseLda)
