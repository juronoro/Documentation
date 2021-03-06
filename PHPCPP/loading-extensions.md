# How does PHP load its extensions?

You probably know that native PHP extensions are compiled into *.so 
files on unix-like systems, and *.dll files in Windows environments, and that
the global php.ini file holds a list of all extensions available on your system.
This means that if you're building your own extension, you are also going to
create such a *.so or *.dll file and you have to update the PHP 
configuration file so that your own extension is loaded by PHP.

## The get_module() startup function

Before we explain how you can create your own extension, we first explain
what PHP does to load an extension. When PHP starts, it loads the *.ini 
configuration file(s) from its configuration directory and for each 
"extension=name.so" line in these config files, it opens the appropriate 
library, and calls the "get_module()" function from it. Each extension library 
(your extension too) must therefore define and implement this "get_module()" 
function. The function is called by PHP right after the library is loaded 
(and thus way before pageviews are handled), and it should return a memory 
address that points to a structure that holds information about all functions, 
classes, variables and constants that are made available by the extension. 

The structure that the get_module() returns is defined in the header files of 
the Zend engine, but it is a pretty complicated structure without good documentation. 
Luckily, the PHP-CPP library makes life easier for you, and offers an Extension 
class that can be used instead.

```cpp
#include <phpcpp.h>

/**
 *  tell the compiler that the get_module is a pure C function
 */
extern "C" {
    
    /**
     *  Function that is called by PHP right after the PHP process
     *  has started, and that returns an address of an internal PHP
     *  strucure with all the details and features of your extension
     *
     *  @return void*   a pointer to an address that is understood by PHP
     */
    PHPCPP_EXPORT void *get_module() 
    {
        // static(!) Php::Extension object that should stay in memory
        // for the entire duration of the process (that's why it's static)
        static Php::Extension myExtension("my_extension", "1.0");
        
        // @todo    add your own functions, classes, namespaces to the extension
        
        // return the extension
        return myExtension;
    }
}
```

In the example above you see a very straightforward implementation of the
get_module() function. Every PHP extension that uses the PHP-CPP library
implements this function in a more or less similar way, and it is the 
starting point of each extension. A number of elements require special attention. 
For a start, the only header file that you see is the phpcpp.h header
file. If you're using the PHP-CPP library to build your own extensions,
you do not have to include the complicated, unstructured, and mostly undocumented
header files of the Zend engine - all you need is this single phpcpp.h header 
file of the PHP-CPP library. If you insist, you are of course free to also 
include the header files of the core PHP engine - but you do not have to. 
PHP-CPP takes care of dealing with the internals of the PHP engine, and offers 
you a simple to use API.

The next thing that you'll notice is that we placed the get_module() function
inside an 'extern "C"' code block. As the name of the library already gives away, 
PHP-CPP is a C++ library. However, PHP expects your library, and especially your 
get_module() function, to be implemented in C and not in C++. That's why we've 
wrapped the get_module() function in an 'extern "C"' block. This will instruct 
the C++ compiler that the get_module() is a regular C function, and that it
should not apply any C++ name mangling to it.

The PHP-CPP library defines a "PHPCPP_EXPORT" macro that should be placed
in front of the get_module() function. This macro makes sure that the get_module()
function is publicly exported, and thus callable by PHP. The macro has a different
implementation based on the compiler and operating system.

This, by the way, is also the only macro that PHP-CPP offers. PHP-CPP intends to 
be a plain C++ library, without using magic or tricks from 
pre-processors. What you see is what you get: If something looks like a 
function, you can be sure that it actually IS a function, and when something 
looks like a variable, you can be sure that it also IS a variable.

Let's move on. Inside the get_module() function the Php::Extension object is 
instantiated, and it is returned. It is crucial that you make a <i>static</i>
instance of this Php::Extension class, because the object must exist for the 
entire lifetime of the PHP process, and not only for the duration of the get_module()
call. The constructor takes two arguments: the name of your extension and 
its version number.

The final step in the get_module() function is that the extension object
is returned. This may seem strange at first, because the get_module() function
is supposed to return a pointer-to-void, and not a full Php::Extension object.
Why does the compiler not complain about this? Well, the Php::Extension class 
has a cast-to-void-pointer-operator. So although it seems that you're returning 
the full extension object, in reality you only return a memory address that
points to a data structure that is understood by the core PHP engine and that
holds all the details of your extension.

Note that the example above does not yet export any native functions or 
native classes to PHP - it only creates the extension. That is going to be 
[the next step](your-first-extension).

