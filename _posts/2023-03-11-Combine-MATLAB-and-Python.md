---
layout:     post
title:      Combine MATLAB and Python
subtitle:   Problem
date:       2023-03-11
author:     UPOJZSB
header-img: img/post-bg-Matlab_Python.jpeg
catalog: true
tags:
    - MATLAB
    - Python
    - Problem
---

# Prologue

I wanted to modify an existing mathematical transform with something new, and write it in MATLAB, but continuous wavelet transform is not well-developed in MATLAB (as I think).

I tried to find a easy-to-use implementation and successfully found a Python package. However, every packages I have to use to generate/analyze data are written in MATLAB, so, I have to call Python function from MATLAB.

***Version: MALTAB R2020B; Python: 3.8.6***

# Use Python in MATLAB

## Call Python Function

Calling Python function is easy. After setting Python environment, just type: `output = py.python_file.python_function(argument1, argument2);`, the function `python_function` in the file (the file is in the same path with the caller MATLAB function) `python_file` with arguments `argument1` and `argument2`, the returned value would be stored in `output`.

### Arguments

#### Data Type

##### Complex Number

Complex number is not supported as a argument, so we should split it into two parts: real part and imaginary part, and put them together in Python:

In MATLAB:
```MATLAB
rt = py.cwt_matlab_interface.cwt_matlab(real(ifpfx), imag(ifpfx), wavelet);
```

In Python:
```python
def cwt_matlab(data_re, data_im, wavelet):
    wavelet = cwt.valid_wavelet[wavelet]
    # Matlab can not transfer a complex matrix to python directly, so we split it into real part and imag part
    # so that we can transfer it. Therefore, we have to re-combine it.
    data = np.array(data_re, dtype='complex128')+1j*np.array(data_im, dtype='complex128')
    return cwt.single_cwt(data, wavelet)
```

### Return Value

#### Data Type

MATLAB accepts `numpy.ndarray` as return value from Python, and we can convert it into MATLAB *double* variable via `double(output)` if the variable name of the return value is `output`.

##### Complex Number

However, like argument, `double` function can not handle complex `ndarray`, so, there are two ways to deal with this problem.

- Split the return value into two parts and handle it in MALTAB (like what we have done in the Arguments-Data Type section)

- Deal it in MATLAB

Here, we tried to deal it in MATLAB.

As the return value from Python NumPy, the data struct of `ndarray` in MATLAB is listed below.

![Data struct Of ndarray](/img/post/230311CMP/230311-CMP-RV-DT-CN1.jpg)

We can see the real part and the imaginary part is in `real` and `imag`, respectively. So, a quite simple way to combine them is:
```MATLAB
rt = double(rt.real)+1j*double(rt.imag);
```

But sometimes, an error will occur:

> Python Error: ValueError: ndarray is not contiguous

Someone says it can be solved in Python via `np.ascontiguousarray`, but it did not solve my problem. I found that after adding a small perturbation (even if the perturbation is literally 0), `double` works. So, we can combine it like this:

```MATLAB
rt = double(rt.real+0)+1j*double(rt.imag+0);
```


## Misc.

### Set Python Environment

Setting Python Environment is necessary before using. It's a easy work and have to do only once. Run:
```MATLAB
pyenv('Version', 'D:\Program Files\Python38\python.exe');
```

The second argument is the path to Python.

### Reload Python Modules

After modifying Python functions/modules, we have to reload the module in MATLAB. Here is the reload function in `cwt_matlab_interface`:

```MATLAB
function clear_py_interface()

warning('off','MATLAB:ClassInstanceExists')
clear classes;
mod = py.importlib.import_module('cwt_matlab_interface');
py.importlib.reload(mod);
```

Note that the parameter that passed to `py.importlib.import_module` **can not** be a string variable, i.e. we can not write this function like this:

```MATLAB
function clear_py_interface(name)

warning('off','MATLAB:ClassInstanceExists')
clear classes;
mod = py.importlib.import_module(name);
py.importlib.reload(mod);
```

In this case, we will receive an error message:

> Reference to a cleared variable name. \
> Error in clear_py_interface (line 5) \
> mod = py.importlib.import_module(name);


## Reference

[How can I clear specific Python classes/modules from memory without using "clear classes"](https://www.mathworks.com/matlabcentral/answers/409159-how-can-i-clear-specific-python-classes-modules-from-memory-without-using-clear-classes#answer_329504)
