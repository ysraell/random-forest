# TODO and bugs.

### BUGS:
- Fix automated version collect from `pyproject.toml`.

### TODO Utils:

- Add methods from [scikit-survival](https://scikit-survival.readthedocs.io/en/stable/user_guide/random-survival-forest.html) for comparison.


### TODO v1.0.3:

- ~~Remove multithread option for concurrency. Is not working properly, not stable.~~
- ~~For avoid `RecursionError` and to have a appropriate depth level in the trees, we need a way to limit the recursive deepening.~~

E.g.:
```python
max_level = 3
def grow_tree(level=0):
    if level >= max_level:
        return 'leaf'
    return {
        f"L{level}" : grow_tree(level+1),
        f"R{level}" : grow_tree(level+1)
    }

Tree = grow_tree()
```
Output:
```json
{
    "L0": {
        "L1": {
            "L2": "leaf",
            "R2": "leaf"
        },
        "R1": {
            "L2": "leaf",
            "R2": "leaf"
        }
    },
    "R0": {
        "L1": {
            "L2": "leaf",
            "R2": "leaf"
        },
        "R1": {
            "L2": "leaf",
            "R2": "leaf"
        }
    }
}
```


Source: [What Is the Maximum Recursion Depth in Python](https://www.codingem.com/python-maximum-recursion-depth/), Artturi Jalli.

### TODO v1.1:

- Fix multithread logic.
- Mssing data issue:
    - Data Imputation using the Forest (with and without true label).
    - Prediction with missing values, approaches:
        - *A*) Use imputation data before prediction. Different from *A*, choose the value with the higher probability.
        - *B*) (User) Set a default value for each feature a priori. When facing a missing value, use the given default value.
- Create new forests from a cross merging between other forests, for a given amount of trees for the output forest:~
    - by optimization, based on a GA and MC approaches, using a given test dataset;
    - Design as a subclass of the `RandomForestMC` for optimization approaches and a function for randomness and sorted merging.
- Create a notebook with [Memray](https://github.com/bloomberg/memray) applied to the model with different datasets.
- Discover how automate the unit tests for test with each compatible minor version.
- Add parameter for limit depth, min sample to leaf, min feature completed cycles.
- Find out how prevent the follow error msg: `cannot take a larger sample than population when 'replace=false'`. It's maybe interesting to have duplicate rows because during the growing the tree we may consider a different structure of decision reusing values (feature). In fact, the algorithm will prevent the creation of a duplicated decision node. We may set as a input parameter (boolean), or in the same way but with a third parameter to change to `True` when we got a error. Consider a possible performance decreasing this third parameter (using `try` may works?).
- Add parameter for active validation score (like loss) for each set of a given number of trees generated.
- Add a set of functions for generate perfomance metrics: like trees generation/validation time.
- Review the usage of `defaultdict` and try use `dict.setdefault`.
- Review the use of the threshold (TH) for validation process. How we could set the dynamic portion? How spread the TH? The set of rows, for a set of rounds, not reaching the TH, drop and get next? Dynamicaly decreasing the TH for each N sets of rows without sucess?
- Docstring.

### TODO V2.0:

- Extender for predict by regression.
- Refactor to use NumPy or built in Python features as core data operations.
- Tree management framework: to remove or add new trees, version management for set of trees.

### TODO Extras:
- Automated tests using GitHub Actions.
- Develop a Scikit-Learn full compatible model. Ref.: <https://scikit-learn.org/stable/developers/develop.html>.
- Write and publish a article.
- Read the Scikit-learn governance and decision-making (https://scikit-learn.org/stable/governance.html#governance).
