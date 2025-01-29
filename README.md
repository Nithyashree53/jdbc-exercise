File1.html
&lt;!DOCTYPE html&gt;
&lt;html lang=&quot;en&quot;&gt;
&lt;head&gt;
&lt;meta charset=&quot;UTF-8&quot;&gt;
&lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1.0&quot;&gt;
&lt;title&gt;Student Data Submission&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;h2&gt;Submit Student Data&lt;/h2&gt;
&lt;form action=&quot;data&quot; method=&quot;post&quot;&gt;
&lt;label for=&quot;stu_id&quot;&gt;Student ID:&lt;/label&gt;
&lt;input type=&quot;number&quot; id=&quot;stu_id&quot; name=&quot;stu_id&quot; required&gt;&lt;br&gt;&lt;br&gt;
&lt;label for=&quot;stu_name&quot;&gt;Student Name:&lt;/label&gt;
&lt;input type=&quot;text&quot; id=&quot;stu_name&quot; name=&quot;stu_name&quot; required&gt;&lt;br&gt;&lt;br&gt;
&lt;label for=&quot;stu_dept&quot;&gt;Student Department:&lt;/label&gt;
&lt;input type=&quot;text&quot; id=&quot;stu_dept&quot; name=&quot;stu_dept&quot; required&gt;&lt;br&gt;&lt;br&gt;
&lt;label for=&quot;stu_cgpa&quot;&gt;Student CGPA:&lt;/label&gt;
&lt;input type=&quot;number&quot; id=&quot;stu_cgpa&quot; name=&quot;stu_cgpa&quot; step=&quot;0.1&quot;
required&gt;&lt;br&gt;&lt;br&gt;
&lt;button type=&quot;submit&quot;&gt;Submit&lt;/button&gt;
&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;
Data.java

package ab;
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
/**
* Servlet implementation class data
*/
@WebServlet(&quot;/data&quot;)
public class data extends HttpServlet {
private static final long serialVersionUID = 1L;
protected void doPost(HttpServletRequest request, HttpServletResponse
response) throws ServletException, IOException {
response.setContentType(&quot;text/html&quot;);
PrintWriter out = response.getWriter();
String stuId = request.getParameter(&quot;stu_id&quot;);

String stuName = request.getParameter(&quot;stu_name&quot;);
String stuDept = request.getParameter(&quot;stu_dept&quot;);
String stuCgpa = request.getParameter(&quot;stu_cgpa&quot;);
Connection connection = null;
PreparedStatement preparedStatement = null;
try {
Class.forName(&quot;com.mysql.cj.jdbc.Driver&quot;);
connection =
DriverManager.getConnection(&quot;jdbc:mysql://localhost:3306/student&quot;,
&quot;priya&quot;,&quot;system&quot;);
String sql = &quot;INSERT INTO student (stu_id, stu_name, stu_dept,
stu_cgpa) VALUES (?, ?, ?, ?)&quot;;
preparedStatement = connection.prepareStatement(sql);
preparedStatement.setInt(1, Integer.parseInt(stuId));
preparedStatement.setString(2, stuName);
preparedStatement.setString(3, stuDept);
preparedStatement.setInt(4, Integer.parseInt(stuCgpa));
int rowsInserted = preparedStatement.executeUpdate();
out.println(&quot;&lt;html&gt;&quot;);
out.println(&quot;&lt;head&gt;&lt;title&gt;Data Submission&lt;/title&gt;&lt;/head&gt;&quot;);
out.println(&quot;&lt;body&gt;&quot;);
if (rowsInserted &gt; 0) {
out.println(&quot;&lt;h2&gt;Data Submitted Successfully&lt;/h2&gt;&quot;);
out.println(&quot;&lt;p&gt;Student ID: &quot; + stuId + &quot;&lt;/p&gt;&quot;);
out.println(&quot;&lt;p&gt;Student Name: &quot; + stuName + &quot;&lt;/p&gt;&quot;);
out.println(&quot;&lt;p&gt;Student Department: &quot; + stuDept + &quot;&lt;/p&gt;&quot;);
out.println(&quot;&lt;p&gt;Student CGPA: &quot; + stuCgpa + &quot;&lt;/p&gt;&quot;);
} else {
out.println(&quot;&lt;h2&gt;Failed to Submit Data&lt;/h2&gt;&quot;);
}
out.println(&quot;&lt;/body&gt;&quot;);
out.println(&quot;&lt;/html&gt;&quot;);
} catch (Exception e) {
e.printStackTrace();
out.println(&quot;&lt;h2&gt;Error Occurred: &quot; + e.getMessage() + &quot;&lt;/h2&gt;&quot;);
} finally {
try {
if (preparedStatement != null) preparedStatement.close();
if (connection != null) connection.close();
} catch (Exception e) {
e.printStackTrace();
}
}
}
}
