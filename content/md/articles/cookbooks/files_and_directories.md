{:title "Working with Files and Directories in Clojure"
 :layout :page :page-index 4400}

This cookbook covers working with files and directories from Clojure,
using functions in the `clojure.java.io` namespace as well as parts of
the JDK via interoperability.

This work is licensed under a <a rel="license"
href="http://creativecommons.org/licenses/by/3.0/">Creative Commons
Attribution 3.0 Unported License</a> (including images &
stylesheets). The source is available [on
Github](https://github.com/clojure-doc/clojure-doc.github.io).


## Preliminaries

Note that for the examples below, "io" is an alias for
`clojure.java.io`. That is, it's assumed your `ns` macro contains:

``` clojure
(:require [clojure.java.io :as io])
```

or else in the repl you've loaded it:

``` clojure
(require '[clojure.java.io :as io])
```



## Recipes


### Read a file into one long string

``` clojure
(def a-long-string (slurp "foo.txt"))
```

Note, you can pass urls to `slurp` as well. See also [slurp at
Clojuredocs](http://clojuredocs.org/clojure_core/clojure.core/slurp).


### Read a file one line at a time

Suppose you'd like to call `my-func` on every line in a file,
and return the resulting sequence:

``` clojure
(with-open [rdr (io/reader "foo.txt")]
  (doall (map my-func (line-seq rdr))))
```

The `doall` is needed because the `map` call is lazy. The lines that
`line-seq` gives you have no trailing newlines (and empty lines in the
file will yield empty strings ("")).


### Write a long string out to a new file

``` clojure
(spit "foo.txt"
      "A long
multi-line string.
Bye.")
```

Overwrites the file if it already exists. To append, use

``` clojure
(spit "foo.txt" "file content" :append true)
```


### Write a file one line at a time

Suppose you'd like to write out every item in a vector, one item per
line:

``` clojure
(with-open [wrtr (io/writer "foo.txt")]
  (doseq [i my-vec]
    (.write wrtr (str i "\n"))))
```


### Check if a file exists

``` clojure
(.exists (io/file "filename.txt"))
```

Is it a directory? :

``` clojure
(.isDirectory (io/file "path/to/something"))
```

An io/file is a java.io.File object (a file or a directory). You can
call a number of functions on it, including:

    exists        Does the file exist?
    isDirectory   Is the File object a directory?
    getName       The basename of the file.
    getParent     The dirname of the file.
    getPath       Filename with directory.
    mkdir         Create this directory on disk.

To read about more available methods, see [the java.io.File
docs](http://docs.oracle.com/javase/7/docs/api/java/io/File.html).


### Get a list of the files and dirs in a given directory

As `File` objects:

``` clojure
(.listFiles (io/file "path/to/some-dir"))
```

Same, but just the *names* (strings), not File objects:

``` clojure
(.list (io/file "path/to/some-dir"))
```

The results of those calls are seqable.


## See also

  * <https://github.com/Raynes/fs>
  * the I/O section of the [cheatsheet](http://clojure.org/cheatsheet)
