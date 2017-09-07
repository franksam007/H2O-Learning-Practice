## H20的各种启动方式
### 简单命令行启动  
<code>java \<JVM Options\> -jar h2o.jar \<H2O Options\></code>
* JVM Options  
  * -Xmx\<Heap Size\>: 设置节点的堆内存，默认为1G
* H2O Options  
  * -h or -help: 显示帮助信息
  * -version: 显示版本信息并退出
  * -name \<H2OCloudName\>: 指定所启动实例所属云的名字，云名字相同的实例会组成一个云（集群）
  * -flatfile \<FileName\>: 指定一个平面文件名，该文件中包含快速建立集群的实例IP地址
  * -ip \<IPnodeAddress\>: 指定H2O实例绑定的IP，默认使用localhost，支持IPv4和IPv6，但不支持IPv6中“::"的简写形式
  * -port \<#\>: 指定REST API使用的端口，通讯端口为此端口号+1
  * -baseport \<#\>: 指定一个用于搜寻Rest API用空闲端口的起始端口号（默认是54322），通讯端口将是此端口号+1
  * -network \<ip_address/subnet_mask\>: Specify an IP addresses with a subnet mask. The IP address discovery code binds to the first interface that matches one of the networks in the comma-separated list; to specify an IP address, use -network. To specify a range, use a comma to separate the IP addresses: -network 123.45.67.0/22,123.45.68.0/24. For example, 10.1.2.0/24 supports 256 possibilities. IPv4 and IPv6 addresses are supported.  
    * IPv4: -network 178.0.0.0/8  
    * IPv6: -network 2001:db8:1234:0:0:0:0:0/48 (short version of IPv6 with :: is not supported.)  
  * -ice_root \<fileSystemPath\>: Specify a directory for H2O to spill temporary data to disk (where <fileSystemPath> is the file path).
  * -log_dir \<fileSystemPath\>: Specify the directory where H2O writes logs to disk. (This usually has a good default that you need not change.
  * -log_level \<TRACE,DEBUG,INFO,WARN,ERRR,FATAL\>: Specify to write messages at this logging level, or above. The default is INFO.
  * -flow_dir \<server-side or HDFS directory\>: Specify a directory for saved flows. The default is /Users/h2o-<H2OUserName>/h2oflows (where <H2OUserName> is your user name).
  * -nthreads \<#ofThreads\>: Specify the maximum number of threads in the low-priority batch work queue (where <#ofThreads> is the number of threads).
  * -client: Launch H2O node in client mode. This is used mostly for running Sparkling Water.
  * -context_path <context_path>: The context path for jetty.
### 建立集群
New H2O nodes join to form a cloud during launch. After a job has started on the cloud, it prevents new members from joining.

* To start an H2O node with 4GB of memory and a default cloud name:  
  <code>java -Xmx4g -jar h2o.jar</code>
* To start an H2O node with 6GB of memory and a specific cloud name:  
  <code>java -Xmx6g -jar h2o.jar -name MyCloud</code>
* To start an H2O cloud with three 2GB nodes using the default cloud names:  
  <code>java -Xmx2g -jar h2o.jar &   java -Xmx2g -jar h2o.jar &   java -Xmx2g -jar h2o.jar &</code>
  
Wait for the <code>INFO: Registered: # schemas in: #mS</code> output before entering the above command again to add another node (the number for # will vary).

H2O provides two modes for cluster creation:

  * 基于多播（Multicast based）
  * 基于平面配置文件（Flatfile based）
#### 多播
In this mode, H2O is using IP multicast to announce existence of H2O nodes. 
Each node selects the same multicast group and port based on specified shared cloud name 
(see -name option). For example, for IPv4/PORT a generated multicast group is 
228.246.114.236:58614 (for cloud name michal), for IPv6/PORT a generated multicast group is 
ff05:0:3ff6:72ec:0:0:3ff6:72ec:58614 (for cloud name michal and link-local address which enforce link-local scope).

For IPv6 the scope of multicast address is enforced by a selected node IP. For example, 
if IP the selection process selects link-local address, then the scope of multicast will be link-local. 
This can be modified by specifying JVM variable sys.ai.h2o.network.ipv6.scope which enforces addressing 
scope use in multicast group address (for example, -Dsys.ai.h2o.network.ipv6.scope=0x0005000000000000 
enforces the site local scope. For more details please consult the class water.util.NetworkUtils).

For more information about scopes, see the following image.

#### 平面文件
The flatfile describes a topology of a H2O cluster. The flatfile definition is passed via the -flatfile option.
It needs to be passed at each node in the cluster, but definition does not be the same at each node. However, 
transitive closure of all definitions should contains all nodes. For example, for the following definition

<table>
  <tr>
    <td>Nodes</td>
    <td>nodeA</td>
    <td>nodeB</td>
    <td>nodeC</td>
   </tr>
   <tr>
    <td>Flatfile</td>
    <td>A,B</td>
    <td>A, B</td>
    <td>B, C</td>
   </tr>
 </table>
 
The resulting cluster will be formed by nodes A, B, C. The node A transitively sees node C via node B flatfile definition, 
and vice versa.

The flatfile contains a list of nodes in the form IP:PORT that are going to compose a resulting cluster 
(each node on a separated line, everything prefixed by # is ignored). Running H2O on a multi-node cluster
allows you to use more memory for large-scale tasks (for example, creating models from huge datasets) than
would be possible on a single node.

  *IPv4:

  <pre><code># run two nodes on 108   
  10.10.65.108:54322   
  10.10.65.108:54325</code></pre>

  *IPv6:

  <pre><code>0:0:0:0:0:0:0:1:54321   
  0:0:0:0:0:0:0:1:54323</code></pre>
  
### 从Python启动
Use the h2o.init() function to initialize H2O. This function accepts the following options. Note that in most cases, simply using h2o.init() is all that a user is required to do.

 * url: Full URL of the server to connect to. (This can be used instead of ip + port + https.)
 * ip: The ip address (or host name) of the server where H2O is running.
 * port: Port number that H2O service is listening to.
 * https: Set to True to connect via https:// instead of http://.
 * insecure: When using https, setting this to True will disable SSL certificates verification.
 * username: The username to log in with when using basic authentication.
 * password: The password to log in with when using basic authentication.
 * cookies: Cookie (or list of) to add to each request.
 * proxy: The proxy server address.
 * start_h2o: If False, do not attempt to start an H2O server when a connection to an existing one failed.
 * nthreads: “Number of threads” option when launching a new H2O server.
 * ice_root: The directory for temporary files for the new H2O server.
 * enable_assertions: Enable assertions in Java for the new H2O server.
 * max_mem_size: Maximum memory to use for the new H2O server.
 * min_mem_size: Minimum memory to use for the new H2O server.
 * strict_version_check: If True, an error will be raised if the client and server versions don’t match.
