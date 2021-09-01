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
  
