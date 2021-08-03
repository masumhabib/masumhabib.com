Debugging boost.python using GDB
################################
:date: 2014-11-02 23:09
:author: masum
:category: Uncategorized
:slug: 1481
:status: draft

| gdb
|  (gdb) target exec python
|  (gdb) run
|  (gdb) from qmicad import \*
|  (gdb) break qmicad::python::npy2cxmat(boost::python::api::object)
