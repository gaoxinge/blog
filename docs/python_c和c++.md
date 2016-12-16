具体可参见

- [Python与C/C++混合编程](https://zhuanlan.zhihu.com/p/20150641?refer=python-dev)
- [聊聊Python ctypes模块](https://zhuanlan.zhihu.com/p/20152309?refer=python-dev)

## c/c++调用Python（基础篇）

代码

```c
//my_python.c
#include <Python.h>

int main(int argc, char *argv[])
{
	Py_SetPythonHome("C:/Python27");
	Py_SetProgramName(argv[0]);
	Py_Initialize();
	PyRun_SimpleString("print 'Hello Python!'\n");
	Py_Finalize();
	return 0;
}
```

编译

```
cl my_python.c -I C:\Python27\include C:\Python27\libs\python27.lib
```

运行

```
Hello Python!
```

代码

```python
#great_module.py
def great_function(a):
    return a + 1
```

```c
//my_python.c
#include <Python.h>
int great_function_from_python(int a);

int great_function_from_python(int a) 
{
    int res;
    PyObject *pModule, *pFunc;
    PyObject *pArgs, *pValue;
    
    // import
    pModule = PyImport_Import(PyString_FromString("great_module"));

    // great_module.great_function
    pFunc = PyObject_GetAttrString(pModule, "great_function"); 
    
    // build args
    pArgs = PyTuple_New(1);
    PyTuple_SetItem(pArgs,0, PyInt_FromLong(a));
      
    // call
    pValue = PyObject_CallObject(pFunc, pArgs);
    
    res = PyInt_AsLong(pValue);
    return res;
}

int main(int argc, char *argv[])
{
	Py_SetPythonHome("C:/Python27");
	Py_Initialize();
	printf("%d", great_function_from_python(2));
	Py_Finalize();
	return 0;
}
```

编译

```
cl my_python.c -I C:\Python27\include C:\Python27\libs\python27.lib
```

运行

```
3
```

## Python调用c/c++（基础篇）

代码

```c
//great_module.c
#include <Python.h>

int great_function(int a) {
    return a + 1;
}

static PyObject * _great_function(PyObject *self, PyObject *args)
{
    int _a;
    int res;

    if (!PyArg_ParseTuple(args, "i", &_a))
        return NULL;
    res = great_function(_a);
    return PyLong_FromLong(res);
}

static PyMethodDef GreateModuleMethods[] = {
    {
        "great_function",
        _great_function,
        METH_VARARGS,
        ""
    },
    {NULL, NULL, 0, NULL}
};

PyMODINIT_FUNC initgreat_module(void) {
    (void) Py_InitModule("great_module", GreateModuleMethods);
}
```

编译

```
cl /LD great_module.c /o great_module.pyd -I C:\Python27\include C:\Python27\libs\python27.lib
```

运行

```python
>>> from great_module import great_function 
>>> great_function(2)
3L
```

## Python调用c/c++（高级篇）

### ctypes

```c
//great_module.c
#include <nmmintrin.h>

#ifdef _MSC_VER
    #define DLL_EXPORT __declspec( dllexport ) 
#else
    #define DLL_EXPORT
#endif

DLL_EXPORT int great_function(unsigned int n) {
    return _mm_popcnt_u32(n);
}
```

```python
>>> from ctypes import *
>>> great_module = cdll.LoadLibrary('./great_module.dll')
>>> print great_module.great_function(13)
```

### 基本类型

```python
from ctypes import *
from platform import *

cdll_names = {
            'Darwin': 'libc.dylib',
            'Linux': 'libc.so.6',
            'Windows': 'msvcrt.dll'
    }

clib = cdll.LoadLibrary(cdll_names[system()])

# 1
clib.printf(c_char_p("Hello %d %f"), c_int(15), c_double(2.3))

# 2
str_format = c_char_p()
int_val = c_int()
double_val = c_double()
str_format.value = "Hello %d %f"
int_val.value = 15
double_val.value = 2.3
clib.printf(str_format,int_val,double_val)
```

```python
from ctypes import *
from platform import *

cdll_names = {
            'Darwin': 'libc.dylib',
            'Linux': 'libc.so.6',
            'Windows': 'msvcrt.dll'
    }

clib = cdll.LoadLibrary(cdll_names[system()])

# 1
s1 = c_char_p('a')
s2 = c_char_p('b')
s3 = clib.strcat(s1, s2)
print s1.value

# 2
s1 = c_char_p('a')
s3 = clib.strcat(s1, 'b')
print s1.value

# 3
s3 = clib.strcat('a', 'b')
print s3

# 4
clib.strcat.restype = c_char_p
s4 = clib.strcat('c', 'd')
print s4
```

### 数组

```c
//great_module.c

#ifdef _MSC_VER
    #define DLL_EXPORT __declspec( dllexport ) 
#else
    #define DLL_EXPORT
#endif 

DLL_EXPORT int array_get(int a[], int index) {
    return a[index];
}
```

```python
>>> from ctypes import *
>>> great_module = cdll.LoadLibrary('./great_module.dll')
>>> type_int_array_10 = c_int * 10
>>> my_array = type_int_array_10()
>>> my_array[2] = c_int(5)
>>> print great_module.array_get(my_array,2)
```

```python
from ctypes import *

type_int_array_10 = c_int * 10
my_array = type_int_array_10()
my_array[2] = c_int(5)
```

```python
from ctypes import *

type_int_array_10 = c_int * 10
type_int_array_10_10 = type_int_array_10 * 10
my_array = type_int_array_10_10()
my_array[1][2] = 3
print my_array[1][2]
```

### 指针

```python
from ctypes import *

type_p_int = POINTER(c_int)
v = c_int(4)
p_int = type_p_int(v)
print p_int[0]
print p_int.contents
```

```python
from ctypes import *

type_p_int = POINTER(c_int)
v = c_int(4)
p_int = pointer(v)
print type(p_int)
print p_int[0]
print p_int.contents
```

### 函数指针

```python
from ctypes import *
from platform import *

cdll_names = {
            'Darwin' : 'libc.dylib',
            'Linux'  : 'libc.so.6',
            'Windows': 'msvcrt.dll'
        }

clib = cdll.LoadLibrary(cdll_names[system()])
CMPFUNC = CFUNCTYPE(c_int, POINTER(c_int), POINTER(c_int))

def py_cmp_func(a, b):
    print type(a)
    print 'py_cmp_func', a[0], b[0]
    return a[0] - b[0]

type_array_5 = c_int * 5
ia = type_array_5(5, 1, 7, 33, 99)
clib.qsort(ia, len(ia), sizeof(c_int), CMPFUNC(py_cmp_func))
```

```python
from ctypes import *
from ctypes import wintypes

WNDENUMPROC = WINFUNCTYPE(wintypes.BOOL, wintypes.HWND, wintypes.LPARAM)
user32 = windll.LoadLibrary('user32.dll')

def EnumWindowsProc(hwnd, lParam):
    length = user32.GetWindowTextLengthW(hwnd) + 1
    buffer = create_unicode_buffer(length)
    user32.GetWindowTextW(hwnd, buffer, length)
    print buffer.value
    return True

user32.EnumWindows(WNDENUMPROC(EnumWindowsProc), 0)
```