# Example for [issue-318](https://github.com/jmcnamara/libxlsxwriter/issues/318)

The repository consists only of
1. the submodule [libxlsxwriter](https://github.com/ANaumann85/libxlsxwriter) at Release_1.0.0.
2. one CMakeLists.txt which adds the submodule as additional folder.

The submodule creates a target, which represents the library xlsxwriter. But the actual name of the target is different between the first and all later cmake runs. To make that behavior clear, the CMakeLists.txt prints either "has the target xlsxwriter" (in the first run) or "has the target testIssue_318" (in the second run).

To explain that behavior, we consider the content  and the type of the variable PROJECT_NAME in two lines:

| file | line || content | type || content | type | 
| CMakeLists.txt | 3 || testIssue_318 | ordinary || testIssue_318 | cache |
| libxlsxwriter/CMakeLists.txt | 96 || testIssue_318 | ordinary || testIssue_318 | cache |
| libxlsxwriter/CMakeLists.txt | 98 || libxlsxwriter | cache || testIssue_318 | cache

That behavior is explained in the [cmake documentation](https://cmake.org/cmake/help/latest/command/set.html#set-cache-entry) 
```
If the cache entry does not exist prior to the call or the FORCE option is given then the cache entry will be set to the given value. Furthermore, any normal variable binding in the current scope will be removed to expose the newly cached value to any immediately following evaluation.
```
