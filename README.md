# Example for [issue-318](https://github.com/jmcnamara/libxlsxwriter/issues/318)

## repository content
The repository consists only of
1. the submodule [libxlsxwriter](https://github.com/ANaumann85/libxlsxwriter) at Release_1.0.0.
2. one CMakeLists.txt which adds the submodule as additional folder.
3. the demo in src/demo.c from the libxlsxwriter repository. But that serves only as an example for compiling and linking.


## The Cause

The problem can be seen with the following commands:
```
mkdir build
cd build
cmake .. 2> err_1
cmake .. 2> err_2
make
```
Due to the second cmake run, the compilation of the demo fails with the error message
``` 
fatal error: xlsxwriter.h: No such file or directory 
```
The (error) output already give hints to the reason for the failure, namely the targets name change.

## Explanations

The CMakeLists.txt in the folder libxlsxwriter creates a target, which represents the library xlsxwriter. But the actual name of the target is different between the first and all later cmake runs. To make that behavior clear, the CMakeLists.txt prints either "has the target xlsxwriter" (in the first run) or "has the target testIssue_318" (in the second run).
Hence the reference in line 15 in CMakeLists.txt does not refer  to the target. 

To explain that behavior, we consider the content  and the type of the variable PROJECT_NAME in two lines:
<table >
  <thead >
    <tr >
      <th rowspan=2 > file </th >
      <th rowspan=2 > line </th >
      <th colspan=2 > first run </th>
      <th colspan=2 > second run </th>
    </tr>
    <tr >
      <th > content </th >
      <th > type </th >
      <th > content </th >
      <th > type </th >
  </thead >
  <tbody >
    <tr > <td > CMakeLists.txt </td > <td > 3 </td > <td > testIssue_318 </td > <td > ordinary </td > <td > testIssue_318 </td > <td > cache  </td > </tr >
    <tr > <td > libxlsxwriter/CMakeLists.txt </td > <td > 96 </td > <td > testIssue_318 </td > <td > ordinary </td > <td > testIssue_318 </td > <td > cache </td > </tr >
    <tr > <td > libxlsxwriter/CMakeLists.txt </td > <td > 98 </td > <td > libxlsxwriter </td > <td > cache </td >  <td > testIssue_318 </td >  <td > cache </td > </tr >
  </tbody >
</table >

That type and value changes are explained in the [cmake documentation](https://cmake.org/cmake/help/latest/command/set.html#set-cache-entry) 
```
If the cache entry does not exist prior to the call or the FORCE option is given then the cache entry will be set to the 
given value. Furthermore, any normal variable binding in the current scope will be removed to expose the newly
cached value to any immediately following evaluation.
```
In our case
1. in the first run, we change the type from ordinary to cache, AND the value from testIssue_318 to xlsxwriter.
2. in the second run, we keep the type AND the value, but from the outer project name.
