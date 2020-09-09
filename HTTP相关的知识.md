### HTTP相关的知识

#### 1、工具

1、安装直接点击链接进入网址的插件

`yarn global add http-server`  yarn自行安装

运行`http-server -c-1`  

**-c-1**：c为缓存，加上-1，就是不缓存

，后面加上网页的名字



#### 2、属性

1. `table-layout`：可以控制表格的居中宽度

   `fix`：根据长度调整，`auto`：根据宽度调整

2. `border-collapse：collapse`合并表格线

3. 一个form表单种，必须要有一个`type=submit`

#### 3、浅谈URL

1. URL包含：协议，域名或IP，端口，路径，查询参数，锚点

   先举个例子

   `https://www.baidu.com/s?wd=hello&rsv_spt=1#5`

   协议：`https://`，或者还有`HTTP`协议

   域名或IP加上端口：`www.baidu.com:443`，端口这里省略

   路径：`/s`

   查询参数：`?wd=hello&rsv_spt=1`

   锚点：`#5`，定位页面的位置

2. DNS的作用是什么，nslookup命令怎么用

   DNS是域名解析，是用来把域名指向网站的IP，其中域名的解析是由DNS服务器来完成

   nslookup的使用：`nslookup baidu.com`

3. IP 的作用是什么，ping 命令怎么用

   提高网络的

4. 域名是什么，分别哪几类域名

   域名是（Domian Name System）的缩写，就是根据域名查出IP地址

   域名分为`主机名，次级域名.顶级域名.根域名`，例如`www.baidu.com.root`

   - 拿百度的域名为例，一般我们时看到的`www.baidu.com`，而这个`root`是对于所有的域名是一样的，所以就省略。
   - 顶级域名（Top-Level domain，缩写TLD）：如`.com`，`.net`
   - 次级域名（second-level domain，缩写为SLD）：如`www.baidu.com`的`baidu`，这一级的域名是可以注册的。
   - 主机名（host）：如`www.baidu.com`的`www`，又称为*三级域名*，这个用户在自己的域里面为服务器分配的名称，是用户可以任意分配的。