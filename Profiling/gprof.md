# Profiling with gprof

We will use the following program to demonstrate the use of ``gprof``.



Execute the following steps to compile the binary with instrumentation information, execute it to generate profiling information (``gmon.out`` by default), and finally run it under ``gprof`` to generate profiling data.

```bash
# compile with -pg flag too generate instrumentation information
$ g++ -Wall -pg vectorofDoubles.cpp -o vectorofDoubles

# execute the binary to generate 'gmon.out'
$ ./vectorofDoubles

# run the binary under gprof (in presence of gmon.out) to write profiling data
$ gprof vectorofDoubles > gprof.txt
```

The sample output generated in ``gprof.txt`` looks like this:

```bash
Each sample counts as 0.01 seconds.
  %   cumulative   self              self     total           
 time   seconds   seconds    calls  ms/call  ms/call  name    
 19.86      0.14     0.14 10000000     0.00     0.00  double& std::vector<double, std::allocator<double> >::emplace_back<double>(double&&)
 15.07      0.26     0.11        1   110.00   190.58  print(std::vector<double, std::allocator<double> >&)
 12.33      0.34     0.09        1    90.00   449.33  insert(std::vector<double, std::allocator<double> >&)
 11.64      0.43     0.09 20000052     0.00     0.00  __gnu_cxx::__normal_iterator<double*, std::vector<double, std::allocator<double> > >::__normal_iterator(double* const&)
 10.96      0.51     0.08                             _init
  4.11      0.54     0.03 20000102     0.00     0.00  __gnu_cxx::__normal_iterator<double*, std::vector<double, std::allocator<double> > >::base() const
  4.11      0.57     0.03 20000000     0.00     0.00  __gnu_cxx::__normal_iterator<double*, std::vector<double, std::allocator<double> > >::operator*() const
  4.11      0.60     0.03 10000000     0.00     0.00  __gnu_cxx::__normal_iterator<double*, std::vector<double, std::allocator<double> > >::operator-(long) const
  3.42      0.62     0.03 10000000     0.00     0.00  __gnu_cxx::__normal_iterator<double*, std::vector<double, std::allocator<double> > >::operator++()
  3.42      0.65     0.03 10000000     0.00     0.00  std::vector<double, std::allocator<double> >::push_back(double&&)
  2.05      0.67     0.01 30000025     0.00     0.00  double&& std::forward<double>(std::remove_reference<double>::type&)
  2.05      0.68     0.01       26     0.58     0.58  std::vector<double, std::allocator<double> >::begin()
```
