### 从Python安装
Run the following commands in a Terminal window to install H2O for Python.  
1. 升级Python, 安装、升级PIP
  * 升级Python到2.7
    * 升级系统依赖包  
       <code>yum -y update</code>
    * 安装开发工具包  
       <code>yum groupinstall "Development tools"</code>
    * 安装其他依赖包  
      <code>sudo yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel</code>
    * 安装Python 2.7  
      <pre><code>$ cd /opt
      $ sudo wget --no-check-certificate https://www.python.org/ftp/python/2.7.6/Python-2.7.6.tar.xz
      $ sudo tar xf Python-2.7.6.tar.xz 
      $ cd Python-2.7.6
      $ sudo ./configure --prefix=/usr/local
      $ sudo make && sudo make altinstall</code></pre>
  * 安装、升级PIP  
    * 安装epel扩展源  
    EPEL(http://fedoraproject.org/wiki/EPEL) 是由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目
    <pre><code>sudo yum -y install epel-release</code></pre>  
    * 安装升级PIP  
    <pre><code>yum -y install python-pip
    pip install --upgrade pip</code></pre>
  
2. 安装依赖包 (prepending with sudo if needed):
     <pre><code>pip install requests
   pip install tabulate
   pip install numpy scipy matplotlib ipython jupyter pandas sympy nose
   pip install scikit-learn
   pip install colorama
   pip install future</code></pre>
  
  _Note_: These are the dependencies required to run H2O. A complete list of dependencies is maintained in the 
  following file: https://github.com/h2oai/h2o-3/blob/master/h2o-py/conda/h2o/meta.yaml

3. 删除以前安装的H2O的Python包  
  <code>pip uninstall h2o</code>

4. Use pip to install this version of the H2O Python module.  
  <code>pip install -f http://h2o-release.s3.amazonaws.com/h2o/latest_stable_Py.html h2o</code>    
  或者直接安装本地已下载的包：  
  <code>pip install h2o-3.14.0.2-py2.py3-none-any.whl</code>     
  
  _Note_: When installing H2O from pip in OS X El Capitan, users must include the --user flag. For example:   
  <code>pip install -f http://h2o-release.s3.amazonaws.com/h2o/latest_stable_Py.html h2o --user</code>

5. （可选）在Python中启动h2o，运行一个DEMO，验证h2o是否工作.
   <pre><code>python
   import h2o
   h2o.init()
   h2o.demo("glm")</code></pre>
   
 6. 用PIP安装本地文件  
   * 安装本地文件  
     <code>pip install /path/to/whl_file/file_name</code>  
   * 使用国内源  
     <code>pip install --trusted-host http://pypi.tuna.tsinghua.edu.cn/simple [要安装的包名]</code>
