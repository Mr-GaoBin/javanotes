# SpringBoot Java导出数据PDF、Excel

git地址：https://gitee.com/chacca/java-upload-pdf.git

>### 配置文件

```java
server:
  port: 80
spring:
  mvc:
    static-path-pattern: /**
  web:
    resources:
      static-locations: classpath:/resources/,classpath:/static/,classpath:/templates/
  datasource:
    url: jdbc:mysql://localhost:3306/demo?serverTime=UTC&characterEncoding=utf8
    username: root
    password: 123456
    type: com.alibaba.druid.pool.DruidDataSource
  thymeleaf:
    suffix: .html
    cache: false
mybatis:
  mapper-locations: classpath:/mybatis/mapper/*.xml
  type-aliases-package: com.example.entity.*
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

>## maven

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.3</version>
        </dependency>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.2</version>
        </dependency>
        <!--   生成PDF需要的依赖    -->
        <dependency>
            <groupId>com.itextpdf</groupId>
            <artifactId>itextpdf</artifactId>
            <version>5.5.10</version>
        </dependency>
        <!--   中文    -->
        <dependency>
            <groupId>com.itextpdf</groupId>
            <artifactId>itext-asian</artifactId>
            <version>5.2.0</version>
        </dependency>
        <!--   excel     -->
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi</artifactId>
            <version>3.9</version>
        </dependency>
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>3.9</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.4</version>
        </dependency>
    </dependencies>
```

>### 工具类

```java
	import com.example.entity.userInfo;
import com.example.service.userInfoService;
import com.itextpdf.text.*;
import com.itextpdf.text.Font;
import com.itextpdf.text.pdf.*;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServletResponse;
import java.io.*;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.List;
public class Util {
    public static void PDF(userInfoService userInfoService, HttpServletResponse response) {
//      使用Windows系统字体
//      BaseFont bf = BaseFont.createFont("C:/WINDOWS/Fonts/SIMYOU.TTF", BaseFont.IDENTITY_H, BaseFont.NOT_EMBEDDED);
//      使用资源字体(路径)
//      BaseFont bf = BaseFont.createFont("/SIMYOU.TTF", BaseFont.IDENTITY_H,BaseFont.NOT_EMBEDDED);
//      使用iTextAsian.jar中的字体
        BaseFont bf = null;
        try {
            bf = BaseFont.createFont("STSongStd-Light", "UniGB-UCS2-H", BaseFont.EMBEDDED);
        } catch (DocumentException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        Font font = new Font(bf, 12);
        ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
        Document document = new Document();
        try {
            PdfWriter.getInstance(document, byteArrayOutputStream);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
        document.open();
        // paragpaph 段落不会换行
        // Chunk 段落会换行
        try {
            document.add(new Paragraph("表格PDF", font));
        } catch (DocumentException e) {
            e.printStackTrace();
        }
        PdfPTable table = new PdfPTable(4);
        //宽度100%填充
        table.setWidthPercentage(100);
        table.setSpacingBefore(3f);
        table.setSpacingAfter(3f);
        ArrayList<PdfPRow> tableRows = table.getRows();
        //设置列宽
        float[] cloumnWidth = {1f, 4f, 2f, 2f};
        try {
            table.setWidths(cloumnWidth);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
        //表头
        PdfPCell[] pdfPCell = new PdfPCell[4];
        PdfPRow pdfPRow = new PdfPRow(pdfPCell);
        pdfPCell[0] = new PdfPCell(new Paragraph("编号", font));
        //水平居中
        pdfPCell[0].setHorizontalAlignment(Element.ALIGN_CENTER);
        //垂直居中
        pdfPCell[0].setVerticalAlignment(Element.ALIGN_MIDDLE);
        pdfPCell[1] = new PdfPCell(new Paragraph("姓名", font));
        pdfPCell[1].setHorizontalAlignment(Element.ALIGN_CENTER);
        pdfPCell[1].setVerticalAlignment(Element.ALIGN_MIDDLE);
        pdfPCell[2] = new PdfPCell(new Paragraph("年龄", font));
        pdfPCell[2].setHorizontalAlignment(Element.ALIGN_CENTER);
        pdfPCell[2].setVerticalAlignment(Element.ALIGN_MIDDLE);
        pdfPCell[3] = new PdfPCell(new Paragraph("性别", font));
        pdfPCell[3].setHorizontalAlignment(Element.ALIGN_CENTER);
        pdfPCell[3].setVerticalAlignment(Element.ALIGN_MIDDLE);
        tableRows.add(pdfPRow);
        List<userInfo> list = userInfoService.QueryAll();
        for (userInfo info : list) {
            PdfPCell[] pdfPCells = new PdfPCell[4];
            PdfPRow pdfPRow1 = new PdfPRow(pdfPCells);
            pdfPCells[0] = new PdfPCell(new Paragraph(String.valueOf(info.getId()), font));
            pdfPCells[0].setHorizontalAlignment(Element.ALIGN_CENTER);
            pdfPCells[0].setVerticalAlignment(Element.ALIGN_MIDDLE);
            pdfPCells[1] = new PdfPCell(new Paragraph(info.getName(), font));
            pdfPCells[1].setHorizontalAlignment(Element.ALIGN_CENTER);
            pdfPCells[1].setVerticalAlignment(Element.ALIGN_MIDDLE);
            pdfPCells[2] = new PdfPCell(new Paragraph(String.valueOf(info.getAge()), font));
            pdfPCells[2].setHorizontalAlignment(Element.ALIGN_CENTER);
            pdfPCells[2].setVerticalAlignment(Element.ALIGN_MIDDLE);
            if (info.getSex() == 0)
                pdfPCells[3] = new PdfPCell(new Paragraph("男", font));
            else pdfPCells[3] = new PdfPCell(new Paragraph("女", font));
            pdfPCells[3].setHorizontalAlignment(Element.ALIGN_CENTER);
            pdfPCells[3].setVerticalAlignment(Element.ALIGN_MIDDLE);
            tableRows.add(pdfPRow1);
        }
        try {
            document.add(table);
        } catch (DocumentException e) {
            e.printStackTrace();
        }
        document.close();
//          实现文件下载
        response.reset();
        try {
            response.addHeader("Content-Disposition", "attachment;filename=" + new String("pdf.pdf".getBytes("gb2312"), StandardCharsets.ISO_8859_1));
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        response.setContentType("application/pdf;charset=utf-8");
        ByteArrayInputStream inputStream = new ByteArrayInputStream(byteArrayOutputStream.toByteArray());
        byte[] bytes = new byte[2048];
        int len = 0;
        while (true) {
            try {
                if (!((len = inputStream.read(bytes)) > 0)) break;
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                response.getOutputStream().write(bytes, 0, len);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        try {
            inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static void Excel(userInfoService service, HttpServletResponse response) {
        Workbook workbook = new XSSFWorkbook();
        Sheet sheet = workbook.createSheet();
        List<userInfo> list = service.QueryAll();
        int count = 0;
        for (userInfo info : list) {
            Row row = sheet.createRow(count);
            int c = 0;
            while (c < 4) {
                switch (c) {
                    case 0:
                        row.createCell(c).setCellValue(info.getId());
                        break;
                    case 1:
                        row.createCell(c).setCellValue(info.getName());
                        break;
                    case 2:
                        row.createCell(c).setCellValue(info.getAge());
                        break;
                    case 3:
                        if (info.getSex() == 0)
                            row.createCell(c).setCellValue("男");
                        else row.createCell(c).setCellValue("女");
                        break;
                }
                c++;
            }
            count++;
        }
        response.reset();
        try {
            response.addHeader("Content-Disposition", "Attachment;Filename=" + new String("excel.xlsx".getBytes("gb2312"), StandardCharsets.ISO_8859_1));
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        response.setContentType("application/vnd.ms-excel;charset=utf-8");
        ServletOutputStream outputStream = null;
        try {
            outputStream = response.getOutputStream();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            workbook.write(outputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            outputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

>### controller

```java
import com.alibaba.fastjson.JSON;
import com.example.entity.userInfo;
import com.example.service.userInfoService;
import com.example.util.Util;
import com.google.gson.Gson;
import com.itextpdf.text.DocumentException;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.util.HashMap;
import java.util.List;
@Controller
public class Pagecontroller {
    @Autowired
    private userInfoService userInfoService;
    @GetMapping("/")
    public String index(){
        return "index";
    }
    @GetMapping("getData")
    @ResponseBody
    public String getData(){
        List<userInfo> userInfos = userInfoService.QueryAll();
        HashMap<String, Object> map = new HashMap<>();
        map.put("code","0");
        map.put("count",userInfos.size());
        map.put("data",userInfos);
        map.put("msg","");
        return new Gson().toJson(map);
    }
    @GetMapping("downPDF")
    public void downPDF(HttpServletResponse response) throws DocumentException, IOException {
        Util.PDF(userInfoService,response);
    }
    @GetMapping("downExcel")
    public void downExcel(HttpServletResponse response) throws IOException {
        Util.Excel(userInfoService,response);
    }
}
```

>### html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" th:href="@{layui/css/layui.css}">
    <style>
        #btnDiv{
            float: right;
        }
    </style>
</head>
<body>
<table class="layui-hide" id="test"></table>
<div id="btnDiv">
<button type="button" class="layui-btn layui-btn-normal" onclick="downPDF()">导出表格PDF</button>
<button type="button" class="layui-btn layui-btn-normal" onclick="downExcel()">导出Excel</button>
</div>
<script th:src="@{layui/layui.js}"></script>
<script th:src="@{https://code.jquery.com/jquery-3.1.1.min.js}"></script>
<script th:inline="none">
    function downPDF(){
        // 创建form表单模拟提交
        var element = document.createElement('form');
        element.action = 'downPDF'
        element.method = 'get'
        document.body.appendChild(element)
        element.submit()
    }
    function downExcel(){
        location.href = 'downExcel'
    }
    layui.use('table', function(){
        var table = layui.table;
        table.render({
            elem: '#test'
            ,url: 'getData'
            ,cols: [[
                {field:'id', title: 'ID'}
                ,{field:'name', title: '姓名'}
                ,{field:'age', title: '年龄'}
                ,{field:'sex', title: '性别'}
            ]]
        });
    });
</script>
</body>
</html>
```

