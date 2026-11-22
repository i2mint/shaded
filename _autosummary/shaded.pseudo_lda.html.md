# shaded.pseudo_lda

Pseudo LDA

### Classes

| [`PseudoLda`](#shaded.pseudo_lda.PseudoLda)(n_components)   | A class finding the hyperplane passing the centers of the classes in y and projecting the vectors in X onto that hyperplane.   |
|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|

### *class* shaded.pseudo_lda.PseudoLda(n_components)

Bases: `BaseEstimator`, `TransformerMixin`

A class finding the hyperplane passing the centers of the classes in y and projecting the vectors in X
onto that hyperplane. The number of components is fixed to n_classes - 1, no more no less. If the classes form
well separated spherical clusters, the class will work as well as LDA but be much faster.
Trouble will arise if the clusters are parallel ellipsoides, close enough that there centers will be relatively
close in the hyperplane chosen by PseudoLda.
