## use `clang-format` in `vscode`
    `c/cpp` 插件默认包含了对`clang-formate`的支持，但是需要每个项目中包含`.clang-format` 文件，此文件为`llvm`根据选项生成，有多个选项可设置。
    - 设置`default formatter` -> `c/cpp (ms-vscode.cpptools)`
    - 设置`c_cpp:Clang_format_style: file`
    - 设置`editor.formatOnSave:true`
