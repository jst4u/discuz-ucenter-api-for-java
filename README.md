# discuz-ucenter-api-for-java
Automatically exported from code.google.com/p/discuz-ucenter-api-for-java
官方技术QQ交流群：200802554 本项目友情赞助：https://me.alipay.com/weifengge
本项目提供完全免费的JAVA版Discuz Ucenter API，可以轻松实现现有JAVA系统与UCenter之间无缝对接。具体实现在的功能如下：

	* 1.单点登录, Discuz! passport for java.
	* 2.基本用户管理的API。

大家的问题很多，但是大多是安装不当的问题,由于时间问题不能一一回复，只要你努力尝试一定能解决。特别要多研究Jsp_demo.jsp这个页面的程序。推荐大家参考以下文档来解决你的问题。
中文名登陆不了的，请将URLEncode.encode(str) 为 URLEncode.encode(str,"GBK")

	* 安装使用方法
	* 登录示例代码
	* 登出示例代码
	* 注册


	* 同步登录的方法介绍

关于uc_api_mysql方式的未实现问题：
暂时不需要这种方式，官方程序默认是这种方式。这种方式将增大集成难度：包括，数据库帐号，权限，驱动设置等。
如果有实在解决不了，我可以提供付费技术支持。
付费服务内容：

	* 1.Ucenter安装配置指导
	* 2.Discuz与JAVA双向登陆集成
	* 3.中文名字登陆问题的
	* 4.免激活方案
	* 5.扩展接口的开发指导与使用支持

集成所需时间：大约30分钟。
费用：500元。

author: tony date :2011.9

------------------------------------------------------------------
UserGuide  
安装使用介绍
Phase-Implementation, Phase-Deploy
Updated Feb 4, 2010 by ping.china
简单介绍长期以来，JAVA开发人员一直找不到好的社区系统，而现在广泛使用的PHP论坛又不能同时使用。 本项目提供了JAVA和Discuz! Ucenter的基本API接口， 你可以在此基础上集成你的应用。
安装方法第一步：UCenter 添加应用

	* 应用名称: [你的系统名称]
	* 接口 URL: [你的应用地址] etc: http://yourhost:80/context/
	* 应用 IP: [你的应用服务器的IP地址]
	* 通信密钥: 123456[随便设]，并将这个值考到config.properties里的UC_KEY

第二步：客户端配置
UC_API = http://localhost/ucUC_IP = 127.0.0.1UC_KEY = 123456UC_APPID = 3UC_CONNECT =第三步：启动客户端
将应用接口发布服务器上。启动。 注意：web.xml 中必须含有：
<servlet>

<servlet-name>

api

</servlet-name>

<servlet-class>

com.fivestars.interfaces.bbs.api.UC

</servlet-class>

<load-on-startup>

2

</load-on-startup>

</servlet>

<servlet-mapping>

<servlet-name>

api

</servlet-name>

<url-pattern>

/api/uc.php

</url-pattern>

</servlet-mapping>

第四步：
运行测试程序： http://localhost/context/Jsp_demo.jsp
结束！
祝你好运！

no_tenAT163.com QQ:18786721

------------------------------------------------------------------

login  
Updated Feb 4, 2010 by ping.china
Client e = new Client(); String result = e.uc_user_login("username", "password");LinkedList[String> rs = XMLHelper.uc_unserialize(result); if(rs.size()>0){
int $uid = Integer.parseInt(rs.get(0)); String $username = rs.get(1); String $password = rs.get(2); String $email = rs.get(3); if($uid > 0) {
System.out.println("登录成功"); System.out.println($username); System.out.println($password); System.out.println($email);String $ucsynlogin = e.uc_user_synlogin($uid); System.out.println("登录成功"+$ucsynlogin);//本地登陆代码 //TODO ... ....} else if($uid == -1) {
System.out.println("用户不存在,或者被删除");
} else if($uid == -2) {
System.out.println("密码错");
} else {
System.out.println("未定义");
}
}else{
System.out.println("Login failed"); System.out.println(result);
}

------------------------------------------------------------------------------------
logout  
logout by using ucenter client
Updated Feb 4, 2010 by ping.china
Client uc = new Client();//setcookie('Example_auth', '', -86400);// 生成同步退出的代码
String $ucsynlogout = uc.uc_user_synlogout(); System.out.println("退出成功"+$ucsynlogout);

--------------------------------------------------------------------------------------------------------------


register  
如何使用API实现注册用户.
Updated Feb 4, 2010 by ping.china
Client uc = new Client();//setcookie('Example_auth', '', -86400);// 生成同步退出的代码
String $returns = uc.uc_user_register("cccc", "ccccc" ,"ccc@abc.com" ); int $uid = Integer.parseInt($returns); if($uid <= 0) {
if($uid == -1) {
System.out.print("用户名不合法");
} else if($uid == -2) {
System.out.print("包含要允许注册的词语");
} else if($uid == -3) {
System.out.print("用户名已经存在");
} else if($uid == -4) {
System.out.print("Email 格式有误");
} else if($uid == -5) {
System.out.print("Email 不允许注册");
} else if($uid == -6) {
System.out.print("该 Email 已经被注册");
} else {
System.out.print("未定义");
}
} else {
System.out.println("OK:"+$returns);
}


----------------------------------------------------------------------------------

syslogging  
同步登陆的方法
Updated Sep 12, 2011 by ping.china
Introduction同步登陆实现方法介绍
Details现找到UC.java
里面有一个方法
protected String doAnswer(HttpServletRequest request, HttpServletResponse response){
     //处理
     String $code = request.getParameter("code");
     if($code==null) return API_RETURN_FAILED;

     //....
}

所有同步操作在此方法中实现。
你可以根据同步的“代码”设定执行方法来实现你的功能。（这部分工作必须由你来实现）
                } else if($action.equals("synlogin")) {

                        if(!API_SYNLOGIN ) return (API_RETURN_FORBIDDEN);

                        doLogin(request, response, $get);


                } else if($action.equals("synlogout")) {

                        if(!API_SYNLOGOUT ) return (API_RETURN_FORBIDDEN);

                        doLogout(request, response, $get);
}

//添加如下方法：并实现他
       
    protected void doLogin(HttpServletRequest request, HttpServletResponse response,Map<String,String> $get){
   
        }
    protected void doLogout(HttpServletRequest request, HttpServletResponse response,Map<String,String> $get){
       
        }
    protected void doUpdatePwd(HttpServletRequest request, HttpServletResponse response,Map<String,String> $get){
       
        }



