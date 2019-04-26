# Oracle兼容性说明 {#concept_188695 .concept}

POLARDB for Oracle高度兼容Oracle，详情如下：

|分类|子类|兼容性说明|
|--|--|-----|
|分区表|PARTITION BY RANGE|兼容|
|PARTITION BY HASH|兼容|
|PARTITION BY LIST|兼容|
|SUB-PARTITIONING|兼容|
|类型|NUMBER|兼容|
|VARCHAR2 , NVARCHAR2|兼容|
|CLOB|兼容|
|BLOB|兼容|
|RAW|兼容|
|LONG RAW|兼容|
|DATE|兼容|
|SQL语法|HIERARCHICAL QUERIES|兼容|
|SYNONYMS \(PUBLIC AND PRIVATE\)|兼容|
|SEQUENCE GENERATOR|兼容|
|HINT|兼容|
|函数|支持个数|3155|
|DUAL|兼容|
|DECODE|兼容|
|ROWNUM|兼容|
|SYSDATE|兼容|
|SYSTIMESTAMP|兼容|
|NVL|兼容|
|NVL2|兼容|
|安全|DATA REDACTION|兼容|
|Database Firewall Only \(SQL/Protect\)|兼容|
|VPD|兼容|
|PL/SQL代码加密|兼容|
|PROFILES FOR PASSWORDS|兼容|
|PL/SQL|PL/SQL Compatible|兼容|
|NAMED PARAMETER NOTATION FOR STORED PROCEDURES|兼容|
|TRIGGERS|兼容|
|REF CURSORS|兼容|
|IMPLICIT / EXPLICIT CURSORS|兼容|
|ANONYMOUS BLOCKS|兼容|
|BULK COLLECT/BIND|兼容|
|ASSOCIATIVE ARRAYS|兼容|
|NESTED TABLES|兼容|
|VARRAYS|兼容|
|PL/SQL SUPPLIED PACKAGES|兼容|
|PRAGMA RESTRICT\_REFERENCES|兼容|
|PRAGMA EXCEPTION\_INIT|兼容|
|PRAGMA AUTONOMOUS\_TRANSACTION|兼容|
|USER DEFINED EXCEPTIONS|兼容|
|OBJECT TYPES|兼容|
|SUB-TYPES|兼容|
|包|支持的包|26|
|包内置函数|317|
|高级功能|DATABASE LINKS|兼容|
|AWR|兼容|
|sql profile|兼容|
|索引推荐|兼容|
|用户级 cpu, memory 资源隔离|兼容|
|TUNING PACKAGE|兼容|
|系统视图|系统视图个数|88|
|C内嵌编程|Pro\*C|兼容|
|客户端驱动|OCI|兼容|

