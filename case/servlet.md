# servlet

### BaseServlet

```java
public class BaseServlet extends HttpServlet {
    /**
     * 当前类继承Bseservlet中的编码格式，根据方法跳转回当前类
     * @param req
     * @param res
     * @throws IOException
     */
    @Override
    public void service(ServletRequest req, ServletResponse res) throws IOException {
        //谁继承Baseservlet谁就是当前类
        Class<? extends BaseServlet> requestClass = this.getClass();
        //设置编码格式
        req.setCharacterEncoding("utf-8");
        res.setCharacterEncoding("utf-8");
        res.setContentType("text/html;charset=utf-8");
        //接收方法名
        String MethodName = req.getParameter("method");
        try {
            //根据方法名得到方法对象
            Method method = requestClass.getMethod(MethodName, HttpServletRequest.class, HttpServletResponse.class);
            //根据方法对象得到方法所在类
            Object invokeValue = method.invoke(this, req, res);
            if (invokeValue != null) {
                req.getRequestDispatcher((String) invokeValue).forward(req, res);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

