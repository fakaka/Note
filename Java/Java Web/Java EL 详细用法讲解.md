  # Java EL 详细用法讲解
  ---
  
  一、EL简介
  
  1.语法结构
  
  ${expression}
  
  2.[]与.运算符
  
  EL 提供.和[]两种运算符来存取数据。
  当要存取的属性名称中包含一些特殊字符，如.或?等并非字母或数字的符号，就一定要使用 []。例如：
  
  ${user.My-Name}应当改为${user["My-Name"] }
  
  如果要动态取值时，就可以用[]来做，而.无法做到动态取值。例如：
  
  ${sessionScope.user[data]}中data 是一个变量
  
  3.变量
  
  EL存取变量数据的方法很简单，例如：${username}。它的意思是取出某一范围中名称为username的变量。因为我们并没有指定哪一个范围的username，所以它会依序从Page、Request、Session、Application范围查找。假如途中找到username，就直接回传，不再继续找下去，但是假如全部的范围都没有找到时，就回传null。
  
  属性范围在EL中的名称
  
  Page PageScope
  Request RequestScope
  Session SessionScope
  Application ApplicationScope
  二、EL隐含对象
  
  1.与范围有关的隐含对象
  
  与范围有关的EL 隐含对象包含以下四个：pageScope、requestScope、sessionScope 和applicationScope；
  
  它们基本上就和JSP的pageContext、request、session和application一样；
  
  在EL中，这四个隐含对象只能用来取得范围属性值，即getAttribute(String name)，却不能取得其他相关信息。
  
  例如：我们要取得session中储存一个属性username的值，可以利用下列方法：
  
  session.getAttribute(“username”) 取得username的值，
  
  在EL中则使用下列方法
  
  ${sessionScope.username}
  
  2.与输入有关的隐含对象
  
  与输入有关的隐含对象有两个：param和paramValues，它们是EL中比较特别的隐含对象。
  
  例如我们要取得用户的请求参数时，可以利用下列方法：
  
  request.getParameter(String name)
  request.getParameterValues(String name)
  在EL中则可以使用param和paramValues两者来取得数据。
  
  ${param.name}
  ${paramValues.name}
  3.其他隐含对象
  
  cookie
  
  JSTL并没有提供设定cookie的动作，例：要取得cookie中有一个设定名称为userCountry的值，可以使用${cookie.userCountry}来取得它。
  
  header和headerValues
  
  header 储存用户浏览器和服务端用来沟通的数据，例：要取得用户浏览器的版本，可以使用${header["User-Agent"]}。
  
  另外在鲜少机会下，有可能同一标头名称拥有不同的值，此时必须改为使用headerValues 来取得这些值。
  
  initParam
  
  initParam取得设定web站点的环境参数(Context)
  
  例：一般的方法String userid = (String)application.getInitParameter(“userid”);
  
  可以使用 ${initParam.userid}来取得名称为userid
  
  pageContext
  
  pageContext取得其他有关用户要求或页面的详细信息。
  
  ${pageContext.request.queryString} 取得请求的参数字符串
  ${pageContext.request.requestURL} 取得请求的URL，但不包括请求之参数字符串
  ${pageContext.request.contextPath} 服务的web application 的名称
  ${pageContext.request.method} 取得HTTP 的方法(GET、POST)
  ${pageContext.request.protocol} 取得使用的协议(HTTP/1.1、HTTP/1.0)
  ${pageContext.request.remoteUser} 取得用户名称
  ${pageContext.request.remoteAddr } 取得用户的IP 地址
  ${pageContext.session.new} 判断session 是否为新的
  ${pageContext.session.id} 取得session 的ID
  ${pageContext.servletContext.serverInfo} 取得主机端的服务信息
  
  三、EL运算符
  
  1.算术运算符有五个：+、-、*或$、/或div、%或mod
  2.关系运算符有六个：==或eq、!=或ne、<或lt、>或gt、<=或le、>=或ge
  3.逻辑运算符有三个：&&或and、||或or、!或not
  4.其它运算符有三个：Empty运算符、条件运算符、()运算符
  
  例：${empty param.name}、${A?B:C}、${A*(B+C)}
  
  四、EL函数(functions)
  
  语法：ns:function( arg1, arg2, arg3 …. argN)
  
  其中ns为前置名称(prefix)，它必须和taglib 指令的前置名称一置
  －－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
  补充：
  
  <%@ taglib prefix="c" uri="http://java.sun.com/jstl/core_rt" %>
   FOREACH:
   <c:forEach items="${messages}"
   var="item"
   begin="0"
   end="9"
   step="1"
   varStatus="var">
   ……
   </c:forEach>
  OUT:
  
  <c:out value="/${logininfo.username}"/>
  c:out>将value 中的内容输出到当前位置，这里也就是把logininfo 对象的username属性值输出到页面当前位置。
  
  ${……}是JSP2.0 中的Expression Language（EL）的语法。它定义了一个表达式，
  
  其中的表达式可以是一个常量（如上），也可以是一个具体的表达语句（如forEach循环体中的情况）。典型案例如下：
  
  ? ${logininfo.username}
  
  这表明引用logininfo 对象的username 属性。我们可以通过“.”操作符引用对象的属性，也可以用“[]”引用对象属性，如${logininfo[username]}与${logininfo.username}达到了同样的效果。
  
  “[]”引用方式的意义在于，如果属性名中出现了特殊字符，如“.”或者“-”，此时就必须使用“[]”获取属性值以避免语法上的冲突（系统开发时应尽量避免这一现象的出现）。与之等同的JSP Script大致如下：
  
  LoginInfo logininfo = (LoginInfo)session.getAttribute(“logininfo”);
   String username = logininfo.getUsername();
  可以看到，EL大大节省了编码量。
  
  这里引出的另外一个问题就是，EL 将从哪里找到logininfo 对象，对于${logininfo.username}这样的表达式而言，首先会从当前页面中寻找之前是否定义了变量logininfo，如果没有找到则依次到Request、Session、Application 范围内寻找，直到找到为止。如果直到最后依然没有找到匹配的变量，则返回null.
  
  如果我们需要指定变量的寻找范围，可以在EL表达式中指定搜寻范围：
  
  ${pageScope.logininfo.username}
  ${requestScope.logininfo.username}
  ${sessionScope.logininfo.username}
  ${applicationScope.logininfo.username}
  在Spring 中，所有逻辑处理单元返回的结果数据，都将作为Attribute 被放置到HttpServletRequest 对象中返回（具体实现可参见Spring 源码中org.springframework.web.servlet.view.InternalResourceView.exposeModelAsRequestAttributes方法的实现代码），也就是说Spring MVC 中，结果数据对象默认都是requestScope。因此，在Spring MVC 中，以下寻址方法应慎用：
  
  ${sessionScope.logininfo.username}
  ${applicationScope.logininfo.username}
  ? ${1＋2}
  结果为表达式计算结果，即整数值3。
  ? ${i>1}
  
  如果变量值i>1的话，将返回bool类型true。与上例比较，可以发现EL会自动根据表达式计算结果返回不同的数据类型。
  表达式的写法与java代码中的表达式编写方式大致相同。
  
  IF / CHOOSE:
  <c:if test=”${var.index % 2 == 0}”>
  *
  </c:if>
  
  判定条件一般为一个EL表达式。
  
  <c:if>并没有提供else子句，使用的时候可能有些不便，此时我们可以通过<c:choose>tag来达到类似的目的：
  
  <c:choose>
   <c:when test="${var.index % 2 == 0}">
   *
   </c:when>
   <c:otherwise>
   !
   </c:otherwise>
   </c:choose>
  类似Java 中的switch 语句，<c:choose>提供了复杂判定条件下的简化处理手法。其中<c:when>子句类似case子句，可以出现多次。上面的代码，在奇数行时输出“*”号，而偶数行时输出“!”。
  －－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
  再补充：
  1 EL表达式用${}表示,可用在所有的HTML和JSP标签中 作用是代替JSP页面中复杂的JAVA代码.
  
  2 EL表达式可操作常量 变量 和隐式对象. 最常用的 隐式对象有${param}和${paramValues}. ${param}表示返回请求参数中单个字符串的值. ${paramValues}表示返回请求参数的一组值.pageScope表示页面范围的变量.requestScope表示请求对象的变量. sessionScope表示会话范围内的变量.applicationScope表示应用范围的变量.
  
  3 <%@ page isELIgnored=”true”%> 表示是否禁用EL语言,TRUE表示禁止.FALSE表示不禁止.JSP2.0中默认的启用EL语言.
  
  4 EL语言可显示 逻辑表达式如${true and false}结果是false 关系表达式如${5>6} 结果是false 算术表达式如 ${5+5} 结果是10
  
  5 EL中的变量搜索范围是:page request session application 点运算符(.)和”[ ]“都是表示获取变量的值.区别是[ ]可以显示非词类的变量
