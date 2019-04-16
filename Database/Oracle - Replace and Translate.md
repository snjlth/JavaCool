
>简要比较： 
- replace 字符串级别的代替 如：SELECT REPLACE('accd','cd','ef') from
  dual;--> aefd
    
- translate 字符级别的代替 如：select translate('acdd','cd','ef') from
  dual; -->aeff
 
> 分别详解 

    Replace
    语法：REPLACE(char,search_string[,replacement_string])
    解释：replace中，每个search_string都被replacement_string所代替
       select replace('acdd','cd','ef') from dual; --> aefd
 
     如果replacement_string为空或为null，那么所有的search_string都被移除
     select replace('acdd','cd','') from dual; --> ad
 
     如果search_string 为null，那么就返回原来的char
     select replace('acdd','ef') from dual; -->acdd
     select replace('acdd','','') from dual; -->acdd(也是两者都为空的情况)
 
 
      translate:
      语法：TRANSLATE('char','from_string','to_string')
      解释：translate中，每个from_string中的字符被to_string中
        
    举例说明：
        
    Sql代码  
    SELECT TRANSLATE('abcdefghij','abcdef','123456') FROM dual; TRANSLATE (   
    --------------   
    123456ghij   
      
    SELECT TRANSLATE('abcdefghij','abcdefghij','123456') FROM dual; TRANSL  
    ----------   
    123456   
     
        
    Sql代码  
    select TRANSLATE('kkaxksx', 'kx', '12') from dual   
      
    结果：11a21s2  
      

    translate中有“#”的特殊用法，以#开头的表示所有字符 
    
    translate的主要作用是提取,替换字符串,其作用有时候和replace差不多.具体看下面的例子
    Sql代码  
    select translate('liyan4h123ui','#liyanhui','#') from dual   
    结果：4123   
      
    select translate('liyan4h123ui','#liyanhui','#z') from dual;   
    结果：z4123   
      
    select translate('liyan4h123ui','#liyanhui','#zx') from dual;   
    结果：zx4123x   
      
    select translate('asadad434323', '#0123456789','#') from dual ;  
    结果：asadad  
     
    
    利用TRANSLATE实现关键字的过滤 
    有时候需要对一些关键词语进行过滤，直接使用replace的话，可能由于这些关键词语比较多而要嵌套使用，语句也不好写，同时也浪费资源。这种情况其实可以使用TRANSLATE和replace组合使用就能完全达到目的了。 
    
    比如要将“深圳”、“北京”等作为关键词语，在显示内容是要将这些词语过滤掉不显示：
     
    Sql代码  
    --首先使用TRANSLATE将关键词语统一转换成一个特殊的字符串，比如这里的X   
      
    SQL> select TRANSLATE('上海北京天津重庆广州深圳武汉','深圳北京','XXXX') from dual;   
    TRANSLATE('上海北京天津重庆广?   
    ------------------------------   
    上海XX天津重庆广州XX武汉   
    --然后用replace将特殊的字符串替换掉。注意：不能用TRANSLATE直接将关键词语直接转换为''字符串   
      
    SQL> select replace(TRANSLATE('上海北京天津重庆广州深圳武汉','深圳北京','XXXX'),'X') from dual;   
    REPLACE(TRANSLATE('上海北京天?   
    ------------------------------   
    上海天津重庆广州武汉   
      
    SQL> --但是，用TRANSLATE是以一个字符为单位的，只要匹配到都会转换。比如不管“北”和“京”是否连接在一起都会做转换   
    SQL> select TRANSLATE('上海京天津重庆北广州深圳武汉','深圳北京','XXXX') from dual;   
    TRANSLATE('上海京天津重庆北广?   
    ------------------------------   
    上海X天津重庆X广州XX武汉   
     
    
    补充：TRANSLATE(string,from,to)转换的两个注意点—— 
    1、转换源字串(from)在目的字串(to)中不存在对应，则转换后被截除 
    2、转换目的字串(to)不能为''，''在oracle中被视为空值，因此无法匹配而返回为空值 
    
    另外，一个汉字作为一个字符还是两个字符进行转换与字符集的设置相关。