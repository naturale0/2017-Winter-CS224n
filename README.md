# CS224n winter 2017
All lecture notes, slides and assignments for [CS224n: Natural Language Processing with Deep Learning](http://web.stanford.edu/class/cs224n/) class by Stanford

The videos of all lectures are available [on YouTube](https://www.youtube.com/playlist?list=PL3FW7Lu3i5Jsnh1rnUwq_TcylNr7EkRe6)

This repository includes all my solutions to assignments and also fix for python 3.6.

# Fix for Python 3.6

I made some changes to Python codes because original codes are based on Python 2.

## For All Assignments:
- Put parenthesis to `print()` function.
- Changed `xrange()` to `range()`
- Changed `import cPickle as pickle`  to `import pickle`

## For Assignment 1
- URL in get_datasets.sh is not working since link to `glove.6B.50d.txt` is dead. You can use below link to download glove data. Also, you can download it from my Github Repository.
    - https://nlp.stanford.edu/data/glove.6B.zip

- In Python 3, You don't have to take care of peculiar encodings in `treebank.py` line 68

    I changed from
    ```python
    sentences += [[w.lower().decode('utf-8').encode('latin-1') for w in splitted]]
    ```
    to
    ```python
    sentences += [[w.lower() for w in splitted]]
    ```


- If you run `$ python q3_run.py`, you will see an error below.
    ```python
    TypeError: write() argument must be str, not bytes
    ```

- This error is raised since `pickle.dump()` and `pickle.load()` do not accept string as input([docs](https://docs.python.org/3/library/pickle.html#pickle.dump)). So, file should be opened in binary mode.
    ```python
    # Error!
    with open("saved_params_%d.npy" % iter, "w") as f:
        pickle.dump(params, f)

    # Good!
    with open("saved_params_$d.npy" % iter, "wb") as f:
        pickle.dump(params, f)
    ```

- In Question 4, `print >> "~~"` is used to write string to file. But this statement is not supported in Python 3, So i changed those lines to `print("~~", file=f)`

## For Assignment 2

- In `/utils/parser_utils.py`, you should change `from general_utils` to `from .general_utils`. I'm not sure why Python cannot find this module, but maybe this error is related to issue with `relative import` or `absolute import`. If you know well about this issue, please let me know.

    ```python
    # Not working!
    from general_utils import logged_loop, get_minibatches

    # Working!
    from .general_utils import logged_loop, get_minibatches
    ```