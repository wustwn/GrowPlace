-- 存储过程
类似于创建一个function(value), 这样在进行相同类操作的时候直接调用定义的Procedure即可。

Sample:
————————————————————————————————————————————————————————————————————————————————————————————————————————
-- 删除存储过程
DROP PROCEDURE IF EXISTS CREATE_USER_DB;

-- 添加; 的转义
DELIMITER ;;

-- 创建存储过程
CREATE PROCEDURE CREATE_USER_DB(IN dbPre VARCHAR(32) CHARSET utf8,IN tableNm VARCHAR(32) CHARSET utf8)
node:BEGIN
        -- 声明变量
        DECLARE i INT DEFAULT 0;
        DECLARE dbname VARCHAR(32) DEFAULT '';
        -- 声明 表名称
        DECLARE tblname VARCHAR(32);

        SET i = 1;
            WHILE i <= 12 DO 
                 
                -- 如果循环到10次，则退出node，不在循环第11和12次。注意配合LEAVE使用 
                IF i = 10 THEN
                    LEAVE node;
                END IF;
                
                
                SET dbname = CONCAT(dbPre,i);
                
                SET tblname = CONCAT(dbname,'.',tableNm);

                -- 创建数据库    
                SET @createDatabasesql = CONCAT('CREATE DATABASE IF NOT EXISTS ',dbname);

                -- 执行动态生成的sql语句
                PREPARE temp FROM @createDatabasesql;
                EXECUTE temp;

                -- 删除表
                SET @delTabl = CONCAT(' DROP TABLE IF EXISTS ',tblname);

                -- 执行动态生成的sql语句
                PREPARE temp FROM @delTabl;
                EXECUTE temp;

                -- 创建表
                SET @createTbsql = CONCAT('create table ',tblname,'(addData datetime,dbname varchar(32) ,username varchar(32) ,age int(3))');

                -- 执行动态生成的sql语句
                PREPARE temp FROM @createTbsql;

                EXECUTE temp;

                -- 增加角标
                SET i = i + 1; 
        END WHILE;
END;
;;
DELIMITER ;

调用：
CALL  CREATE_USER_DB ('DBname','TAbLE');
————————————————————————————————————————————————————————————————————————————————————————————————————————
原文链接：https://blog.csdn.net/yelllowcong/java/article/details/79095130

如果不设计参数传递，可以在存储过程内部直接使用 CREATE TABLE 语句。
