Windows:
1. findstr /M  "str_to_find"  "C:\A\B\src\*"
-- 在C:\A\B\src\所有文件下搜索包含"str_to_find"的文件

2. findstr "str_to_find"  "file.txt"
-- 在file.txt文件内搜索"str_to_find"


Linux:
1. sed -i '/abc/d' a.txt
-- 删除 a.txt 文件中包含 abc 字母的行

2. sed -n '$p' a.txt
-- 输入 a.txt 文件中最后一行。 此命令可以结合判断文件最后是否有空行，进而采取适当的语句插入命令。

3. to uppercases
   -- ${value^^}
   -- ${vaule^^a} bac -> bAc
    
   to lowercases
   -- ${value,,}
 
   keep first work upper
   -- ${value,,}
   -- ${value^}

4. 怎么获得命令执行结果：
  -- cmd 2>&1
  example: 判断apply某patch是否成功，如果命令执行没有输出则正常，有包含error字符串的输出结果则表示失败.
           if (git apply test.patch 2>&1 =~ "error");then
               echo "apply patch error!"
           fi
## Find
1. find . -type d -name "ddd" //查找该目录下名称为ddd的目录
