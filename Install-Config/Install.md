### 从Python安装
Run the following commands in a Terminal window to install H2O for Python.
0. 升级Python, 安装、升级PIP
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
  <pre><code>yum -y install python-pip</code></pre>
  <pre><code>pip install --upgrade pip</code></pre>
  
1. Install dependencies (prepending with sudo if needed):
  <pre><code>pip install requests
pip install tabulate
pip install numpy scipy matplotlib ipython jupyter pandas sympy nose
pip install scikit-learn
pip install colorama
pip install future</code></pre>
  
  _Note_: These are the dependencies required to run H2O. A complete list of dependencies is maintained in the 
  following file: https://github.com/h2oai/h2o-3/blob/master/h2o-py/conda/h2o/meta.yaml

2. Run the following command to remove any existing H2O module for Python.  
  <code>pip uninstall h2o</code>

3. Use pip to install this version of the H2O Python module.  
  <code>pip install -f http://h2o-release.s3.amazonaws.com/h2o/latest_stable_Py.html h2o</code>    
  _Note_: When installing H2O from pip in OS X El Capitan, users must include the --user flag. For example:  
  <code>pip install -f http://h2o-release.s3.amazonaws.com/h2o/latest_stable_Py.html h2o --user</code>

4.  Optionally initialize H2O in Python and run a demo to see H2O at work.
  <pre><code>python
import h2o
h2o.init()
h2o.demo("glm")</code></pre>
