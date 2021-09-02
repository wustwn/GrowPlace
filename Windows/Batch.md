```bat
SET imageStr=

:For
FOR /f "delims=" %%i IN ('docker images') DO SET imageStr=%imageStr% %%i
@REM set "imageStr=%imageStr%"
echo %imageStr%
pause
GOTO Judge

:Judge
IF "%imageStr:gcc=%"=="%imageStr%" (
    GOTO Pull
) ELSE (
    GOTO Finally
)
IF "%imageStr:ubuntu=%"=="%imageStr%"
    GOTO Pull
) ELSE (
    GOTO Finally
)

:Pull
docker pull gcc:9.3.0
docker pull ubuntu:20.04
GOTO Finally

:Finally
IF "%imageStr:wasm-toolchain=%"=="%imageStr%" (
    docker build -t wasm-toolchain:1.0 .
)
GOTO End

:End
```

Explain:
1. 循环获取命令执行结果，暂时windows bat内只能如此处理，每获取一个字符串就设置累加给`imageStr`
  ```bat
  FOR /f "delims=" %%i IN ('docker images') DO SET imageStr=%imageStr% %%i
  ```
  
2. 判断字符串是否包含子字符串，判断`imageStr`是否包含`gcc`字符串
  ```bat
  IF "%imageStr:gcc=%"=="%imageStr%" ()
  ```
  
----
  
- IF
    - file OR filedirectory exsit
    ```bat
    IF not EXSIT file (
        echo No
    ) else (
        echo Yes 
    )
    ```
    
- DEL
    ```bat
    echo on
    DEL "file_path" /s /f /q
    @REM /S      Removes all directories and files in the specified directory in addition to the directory itself.  
    @REM         Used to remove a directory tree.
    @REM /F      Force deletes of read-only files.
    @REM /Q      Dont ask if delete or not
    ```
- Xcopy
   ```bat
   @REM copy source code and head file to mount direcotry .host_mnt
   xcopy /e/y .\include .\.host_mnt\include
   xcopy /e/y .\src .\.host_mnt\src
   @REM /e Copy all subdirectories including empty
   @REM /y 如果“Source”是一个目录或包含通配符，而“Destination”不存在，“xcopy”会假定“destination”指定目录名并创建一个新目录。
   @REN    然后，“xcopy”会将所有指定文件复制到新目录中。默认情况下，“xcopy”将提示您指定“Destination”是文件还是目录。
   ```
