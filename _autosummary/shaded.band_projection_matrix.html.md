# shaded.band_projection_matrix

Band projection matrix

### Functions

| [`buckets_conversion`](#shaded.band_projection_matrix.buckets_conversion)(buckets[, ...])              | Convert buckets in one scale to another through the conversion_function.                                                                                                                                                                                                                                                |
|--------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [`hertz_to_mel`](#shaded.band_projection_matrix.hertz_to_mel)(hertz)                             | convert a frequency given in hertz into mel                                                                                                                                                                                                                                                                             |
| [`make_band_matrix`](#shaded.band_projection_matrix.make_band_matrix)(buckets[, n_freq])             | Given a list of n list of indices, make a matrix of size n by n_freq where the entries of row k are 0 everywhere except at the index in the k list of buckets, where the entries are the inverse of the length of that bucket                                                                                           |
| [`make_band_matrix_row`](#shaded.band_projection_matrix.make_band_matrix_row)(list_entries, row_len)     | Makes a row for the spectral bucket matrix.                                                                                                                                                                                                                                                                             |
| [`make_band_proj_matrix`](#shaded.band_projection_matrix.make_band_proj_matrix)([n_buckets, n_freq, ...]) | Uses make_buckets and make_band_matrix to make a band-based projection matrix                                                                                                                                                                                                                                           |
| [`make_buckets`](#shaded.band_projection_matrix.make_buckets)([n_buckets, freqs_weighting, ...]) | Create greedily buckets starting by aggregating lower frequencies, when the sum of the frequencies values so far exceed the number of buckets created times the target average value for a single bucket, a new bucket is created with or without the last term according to which choice will be closest to the target |
| [`make_buckets_simple`](#shaded.band_projection_matrix.make_buckets_simple)([n_bucket, ...])            |                                                                                                                                                                                                                                                                                                                         |
| [`mel_to_hertz`](#shaded.band_projection_matrix.mel_to_hertz)(mel)                               | convert a frequency given in mel into hertz                                                                                                                                                                                                                                                                             |
| [`visualize_buckets`](#shaded.band_projection_matrix.visualize_buckets)(buckets[, cmap])              | Makes a plot of the buckets using a heatmap                                                                                                                                                                                                                                                                             |

### shaded.band_projection_matrix.buckets_conversion(buckets, conversion_function=<function hertz_to_mel>)

Convert buckets in one scale to another through the conversion_function.
For example, if the conversion_function is set to hertz_to_mel, the function fed with hertz buckets
will return the equivalent mel buckets.

* **Parameters:**
  * **buckets** – list of list of frequencies in one scale
  * **conversion_function** – a mapping from frequencies in one scale to another
* **Returns:**
  the converted buckets

### shaded.band_projection_matrix.hertz_to_mel(hertz)

convert a frequency given in hertz into mel

### shaded.band_projection_matrix.make_band_matrix(buckets, n_freq=1025)

Given a list of n list of indices, make a matrix of size n by n_freq where the entries of row k are 0
everywhere except at the index in the k list of buckets, where the entries are the inverse of the length
of that bucket

* **Parameters:**
  * **buckets** – a list of list containing the indices of non zero entries of the corresponding row
  * **n_freq** – the number of column of the matrix
* **Returns:**
  a len(buckets) by n_freq matrix

```pycon
>>> buckets = make_buckets(n_buckets=15, freqs_weighting=lambda x: np.log(x + 0.001), freq_range=(200, 1000))
>>> M = make_band_matrix(buckets, n_freq=1025)
>>> print(M.shape)
(15, 1025)
```

```pycon
>>> # the matrix sends each of the 200 first unitary vector to the zero vector (not below is not a mathematical proof of that, but solid clue)
>>> vec = [1] * 200 + [0] * (1025 - 200)
>>> print(np.dot(M, vec))
[0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.]
```

```pycon
>>> # after that, we get non zero output (in general at least)
>>> vec = [1] * 200 + [1] + [0] * (1025 - 201)
>>> print(np.dot(M, vec))
[0.01612903 0.         0.         0.         0.         0.
 0.         0.         0.         0.         0.         0.
 0.         0.         0.        ]
```

### shaded.band_projection_matrix.make_band_matrix_row(list_entries, row_len)

Makes a row for the spectral bucket matrix. The row is zero everywhere except on the entries of index in
list_entries where it is 1 / len(list_entries)

* **Parameters:**
  * **list_entries** – the indices of non zero entries of the row
  * **row_len** – the length of the row
* **Returns:**
  an array of length row_len as described above

```pycon
>>> make_band_matrix_row([3], 5)
array([0., 0., 0., 1., 0.])
>>> make_band_matrix_row([0, 3], 5)
array([0.5, 0. , 0. , 0.5, 0. ])
```

### shaded.band_projection_matrix.make_band_proj_matrix(n_buckets=15, n_freq=1025, freqs_weighting=<function <lambda>>, non_empty_bucket=True, reverse=False)

Uses make_buckets and make_band_matrix to make a band-based projection matrix

* **Parameters:**
  * **n_buckets** – final number of buckets
  * **freqs_weighting** – any function assigning a non-negative value to each
    element in the freq_range
    If given a list, the function will be assumed
    to be the one to one mapping the frequencies to the value in the list
  * **n_freq** – the number of column of the matrix
  * **non_empty_bucket** – if set to true, all buckets will have at least one element
  * **reverse** – if set to true, will start aggregating higher frequencies instead
* **Returns:**
  a len(buckets) by n_freq matrix

```pycon
>>> make_band_proj_matrix(n_buckets=3, n_freq=10)
array([[0.14285714, 0.14285714, 0.14285714, 0.14285714, 0.14285714,
        0.14285714, 0.14285714, 0.        , 0.        , 0.        ],
       [0.        , 0.        , 0.        , 0.        , 0.        ,
        0.        , 0.        , 0.33333333, 0.33333333, 0.33333333]])
```

### shaded.band_projection_matrix.make_buckets(n_buckets=15, freqs_weighting=<function <lambda>>, freq_range=(0, 1025), non_empty_bucket=True, reverse=False)

Create greedily buckets starting by aggregating lower frequencies, when the sum of
the frequencies values so far exceed the number of buckets created times the target
average value for a single bucket, a new bucket is created with or without the last
term according to which choice will be closest to the target

* **Parameters:**
  * **n_buckets** – final number of buckets
  * **freqs_weighting** – any function assigning a non-negative value to each
    element in the freq_range
    If given a list, the function will be assumed
    to be the one to one mapping the frequencies to the value in the list
  * **freq_range** – the range of frequencies considered, inclusive on both ends
  * **non_empty_bucket** – if set to true, all buckets will have at least one element
  * **reverse** – if set to true, will start aggregating higher frequencies instead, which for an increasing weight
    function will aggregate the higher frequencies into larger bins as typical
* **Returns:**
  a partition of freq_range

```pycon
>>> buckets = make_buckets(n_buckets=15, freqs_weighting=lambda x: np.log(x + 0.001), freq_range=(200, 1000))
>>> len(buckets) == 15
True
>>> buckets[0][0] # the first bucket starts at the first term of freq_range
200
>>> buckets[-1][-1] # the last bucket ends at the last term of freq_range - 1
999
```

### shaded.band_projection_matrix.make_buckets_simple(n_bucket=10, row_weights_func=<function <lambda>>, n_freq=20)

* **Parameters:**
  * **n_bucket** – int, the number of buckets to create
  * **row_weights_func** – 

    a function, non zero on the integers values from 0 to n_freq. The relative values from
    one integer a to a + 1 will become the relative number of non zero values from row a to
    row a + 1. Note that everything has to be rounded so the actual relationship is a bit more
    complex. The rounding follows two rules:
    1. at least 1 in row
    2. the excess or lack are removed from the end, one unit per index
  * **n_freq** – int, the total number of frequencies, i.e, indices in buckets
* **Returns:**
  a list of list of indices, consecutive, starting at 0 and ending at n_freq

```pycon
>>> make_buckets_simple(row_weights_func=lambda x: 1+x)
[[0], [1], [2], [3], [4, 5], [6, 7], [8, 9, 10], [11, 12, 13], [14, 15, 16], [17, 18, 19, 20]]
```

### shaded.band_projection_matrix.mel_to_hertz(mel)

convert a frequency given in mel into hertz

### shaded.band_projection_matrix.visualize_buckets(buckets, cmap='Spectral')

Makes a plot of the buckets using a heatmap

* **Parameters:**
  **buckets** – a list of the list of indices for each bucket
* **Returns:**
  a plot
