此文件中的内容是利用Python语言

## 1. H2O环境管理  
* 启动与连接   
  <pre><code>import h2o  
  h2o.init()</code></pre>
  
* 列出主要内存对象，即dataframe、model和grid   
  <pre><code>h2o.get_frame(*frame_id*)  
  h2o.get_model(*model_id*)
  h2o.get_grid(*grid_id*)</code></pre>  
  
  下面只取元数据（dataframe)  
  <pre><code>h2o.frame(*frame_id*) 
  h2o.frames()</code></pre>  
  
* 清除内存对象  
  <pre><code>h2o.remove(*object*) 
  h2o.remove_all()</code></pre>
  
## 2. 载入数据与数据对象（DataFrame)   
* 字段类型判断   
  <pre><code>df['*column_name*'].isstring() or df[*column_index*].isstring()
  df['*column_name*'].isnumeric() or df[*column_index*].isnumeric()
  df['*column_name*'].isfactor() or df[*column_index*].isfactor()</code></pre>  
  
* 字段类型转换   
  <pre><code>df['*column_name*'].ascharacter() or df[*column_index*].ascharacter()
  df['*column_name*'].asnumeric() or df[*column_index*].asnumeric()
  df['*column_name*'].asfactor() or df[*column_index*].asfactor()
  df['*column_name*'].as_date(fmt) or df[*column_index*].as_date(fmt)</code></pre>  

* 拷贝DF，生成新的DF（frame_id与原DF不同,如果frame_id与已存在的id相同，会覆盖该DF）  
  <pre><code>*df2* = h2o.deep_copy(*df1*，*new_frame_id*)</code></pre>
   
* DF创建
   1. 原地修改数值，不会创建新的DF，即frame_id不变  
   2. 修改字段类型、DF的拼接和拆分会创建新的DF，即frame_id变化   
  
* 一些限制
  * H2OFrame不支持原地多字符字段的连接，执行后会把h2o的实例搞down
  <pre><code>df['new'] = df['c1'].ascharacter() + df['c2'].ascharacter()</code></pre>
