# 第四章节
## 高级文件处理
### XML的简介
- 数据＋含义 适用于传输数据 而不是显示数据（HTML）
- 可拓展标记语言：意义＋数据
- 标签可自定义 具有自我描述性
- 纯文本表示 跨系统/平台/语言
- W3C标准
#### 常规语法
- 任何的其实标签都有一个结束标签
- 简化写法 <name></name>可以写为<name/>
- 大小写敏感<name>和<Name>不一样
- 每一个文件都要又一个根元素
- 标签必须按合适的顺序进行嵌套 不可错位
- 所有的特性都必须有值 并且在所有的值上面加上引号
- 需要转义字符比如说"<"需要用& l t;(此处把空格去掉)进行代替
- 注释：<!-- 注释内容 -->
#### XML拓展
##### DTD
- DTD定义XML文档的结构
- 使用一系列合法的元素来定义文档结构
- 可嵌套在xml文档中 或者在xml中引用
##### XML Schema
- 定义XML文档的结构 DTD的继承者
- 支持数据类型 可拓展 功能更完善、更强大
- 采用xml编写
##### XSL
- 拓展样式表语言
- XSL作用于XML 等同于CSS作用于HTML
- 内容：
 1. XSLT：转换XML文档
 2. XPath：在XML文档中导航
 3. XSL-FO：格式化XML文档

### XML解析方法
#### DOM
- 树结构
 - DOM:文档对象模型 擅长**小规模的读写**
- 流结构
 - SAX：流机制解释器（推模式） 擅长读
 - Stax：流机制解释器（拉模式） 擅长读 JDK6引入
- DOM：W3C处理XML的标准API
 - 直观易用
 - 其处理方式是将XML**整个**作为类似**树结构**的方式**读入内存**中以便操作及解析 方便修改
 - 解析大数据量的XML文件 会遭遇**内存泄漏及程序崩溃的风险**

- Node节点主接口，getChildNodes方法返回一个NodeList
- NodeList节点列表 每个元素是一个Node
- 节点
 - Document 文档根节点
 - ELement标签节点元素 每一个标签都是标签节点
 - Text节点 包含在XML元素内的 都算Text节点
 - Attr节点 每个属性节点

#### SAX
- 采用事件/流模型来解析XML文档　**更快速　更轻量**
- 有选择的解析和访问　不像DOM加载整个文档　**内存要求较低**
- SAX对XML文档的解析为一次性读取　不创建、不存储文档对象　**很难同时访问文档中的多处数据**
- 推模型（Push）　每当它发现一个节点就引发一个事件　而我们需要编写这些事件的处理程序　关键类DefaultHandler

####Stax
- 流模型中的拉模型
- 在遍历文档时 会把感兴趣的部分从读取器中拉出来 不需要引发事件 允许我们选择性地处理节点 这大大提高了灵活性 以及整体效率
- 两套处理API
 - 基于指针的API XMLStreamReader
 - 基于迭代器的API XMLEventReader


----------
- 以上都是JDK自带的解析功能


----------
- **第三方库**
- JDOM:www.jdom.org
- DOM4J:dom4j.github.io
- 第三方库一般都包含DOM SAX等多种解析方式 是对Java解析进行封装

### JSON简介及概念
- 是一种轻量级的数据交换格式
- 类似XML 更小、更快、更易解析
- 最早用于JavaScript中 容易解析 最后推广到全语言
- 尽管使用JavaScript语法 但是独立于编程语言
- JSONObject和JSONArray
 - **JSON对象：**
 - {"name":"Jo","email":"a@b.com"}
 - 数据在键值对中
 - 数据由逗号分割
 - 花括号保存对象
 - **JSON数组：**
 - 方括号保存数组
 - [{"name":"Jo","email":"a@b.com"},"name":"Jo","email":"a@b.com"] 

#### java的JSON处理
- 由于Java本身没有JSON处理的工具 我们就使用第三方包进行解析
- org.json:JSON官方推荐的解析类
 - 简单易用 通用性强
 - 复杂功能欠缺

- GSON:Google出品
 - 基于反射，可以实现JSON对象 JSON字符串和Java对象互相转换

- Jackson:号称最快的JSON处理器
 - 简单易用 社区更新和发布速度比较快

### JSON主要用途
- JSON生成
- JSON解析
- JSON校验
- 和Java Bean对象进行互解析
 - 具有一个无参的构造函数
 - 可以包括多个属性 所有属性都是private
 - 每个属性都有相应的Getter/Setter方法
 - Java Bean用于封装数据 又可以称为POJO(Plain Old Java Object)

- **注意：**虽然JSON的优点很多 但是XML更加注重标签和顺序 这是JSON所不具有的 是JSON会丢失信息的信息
- JSON会丢失顺序性

### Code
```
import com.google.gson.Gson;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.Result;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.File;
import java.io.IOException;

class Main {

    private static Student student = new Student();
    private static Gson gson = new Gson();

    public static void main(String[] args) {
        readXMLByDom("score.xml");
        writeXMLByDom();

    }

    public static void writeXMLByDom() {
        try {
            DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder documentBuilder = documentBuilderFactory.newDocumentBuilder();

            //create a new document node
            Document document = documentBuilder.newDocument();
            if (document != null) {
                //this is root element
                Element studentDoc = document.createElement("Student");

                //child
                Element name = document.createElement("name");
                name.appendChild(document.createTextNode(student.getName()));
                Element subject = document.createElement("subject");
                subject.appendChild(document.createTextNode(student.getSubject()));
                Element score = document.createElement("score");
                score.appendChild(document.createTextNode(student.getScore()));

                //add the child element
                studentDoc.appendChild(name);
                studentDoc.appendChild(subject);
                studentDoc.appendChild(score);
                TransformerFactory transformerFactory = TransformerFactory.newInstance();
                Transformer transformer = transformerFactory.newTransformer();
                DOMSource source = new DOMSource(studentDoc);

                //create the xml file
                File file = new File("score2.xml");
                Result result = new StreamResult(file);
                transformer.transform(source,result);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void readXMLByDom(String filePath) {
        try {
            DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder documentBuilder = documentBuilderFactory.newDocumentBuilder();
            Document document = documentBuilder.parse(filePath);

            NodeList nodeList = document.getChildNodes();
            for (int i = 0; i < nodeList.getLength(); i++) {
                //get the root element
                Node score = nodeList.item(i);
                
                if (score.getNodeType() == Node.ELEMENT_NODE) {
                    //get the child element one by one
                    NodeList nodeList1 = score.getChildNodes();
                    for (int j = 0; j < nodeList1.getLength(); j++) {
                        Node meta = nodeList1.item(j);
                        
                        //add into the Java bean
                        switch (meta.getNodeName()) {
                            case "name":
                                student.setName(meta.getTextContent());
                            case "subject":
                                student.setSubject(meta.getTextContent());
                            case "score":
                                student.setScore(meta.getTextContent());

                        }
                        //remove all the blank space
                        if (meta.getNodeName() != "#text")
                            System.out.println(meta.getNodeName() + ":" + meta.getTextContent());
                    }
                }
            }
            //POJO to JSON
            String json = gson.toJson(student);
            
            //print JSON
            System.out.println(json);
        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        } catch (SAXException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

## 高级文件处理（续）
- 典型应用
 - 图像文件基本读写
 - 验证码生成
 - 统计图生成
- 图形：Graph
 - 矢量图：根据集合特性来画的 比如点 直线 弧线
 - java.awt包
 - Java 2D库：Graphics2D Line2D Rectangle2D Ellipse2D Arc2D
 - Color Stroke
- **图像：Image**
 - 由像素点组成
 - 格式：jpg png bmp svg wmf gif tiff等
 - 颜色RGB（Red Green Blue）
 - **javax.imageio包**
 - **imageIO BufferedImage ImageReader ImageWriter**

- 关键类描述
 - Java原生支持**jpg png** bmp wbmp gif
- javax.imageio.ImageIO
 - 自动封装多种ImageReader和ImageWriter 读写图像文件
 - read读取图片write写图片

- java.awt.image.BufferedImage 图像在内存中的表示内
 - getHeight获取高度
 - getWidth获取宽度

- 图像文件读写/截取/合并

### 验证码生成
- 验证码 一个图片文件
 - 外框
 - 底色
 - 干扰线：随机产生一些直线
 - 字母
 1. 字母选择，不要0 o 1 I L
 2. 字母颜色（RGB）
 3. 字母位置

### 统计图
- 柱状图/饼图/折线图
- Java原生的Graphics2D可以画 比较繁琐
- 基于jFreeChart(www.jfree.org/jfreechart)可以快速实现统计图生成
 - 设定数据集
 - 调用ChartFactory生成图形

### 总结
- Java的AWT包提供了一些基础的图形工具Graphics2D
- javax的imageio包提供了基础的图像读写和剪辑
- 借助第三方库jFreeChart完成统计类图
- API很多 需要多查询 多练习

### 条形码
- 由宽度不等的多个黑条和白条组成按照一定的编码规则排列
- **通常代表遗传数字 字母 每一位由特殊的含义**
- **一般数据容量30个数字/字母 **
- 专门机构管理：中国物品编码中心

### 二维码
- 比一维码能存储更多的信息 更多数据类型
- 能存储数字/字母/汉字/图片等信息
- 字符集128个字符
- 可以存储击败到几十KB字符
- 抗损坏

### 处理
#### Zxing(Zebra Crossing) Google出品
- 支持1D和2D的Barcode
- 主要类
 - BitMatrix位图矩阵
 - MultiFormatWriter位图编写器
 - MatrixToImageWriter写入图片

#### Barcode4J
- 不是很常用了
- **只负责生成 不负责解析**
- 主要类
- 不能生成QR_Code 需要Zxing支持
 - BarcodeUtil
 - BarcodeGenerator
 - DefaultConfiguration

### docx简介
- 以Microsoft Office的doc/docx为主要处理对象
- word2003之前都是doc 文档格式不公开
- word2007之后都是docx 遵循XML路线 文档格式公开
- **docx为主要研究对象**
- 常见功能：解析 生成（完全生成，模板加部分生成：套打）
- 处理的第三方库
 - Jacob COM4J(windows平台) (免费)
 - **POI** docx4j OpenOffice/Libre Office SDK(免费)
 - Aspose(收费)
 - 一些开源的OpenXML的包

#### POI
- Apache出品 poi.apache.org
- 可处理docx xlsx pptx visio等office
- 纯Java工具包 无序第三方依赖
- 主要类
 - XWPFDocument整个文档对象
 - XWPFParagraph段落
 - XWPFRun 一个片段
 - XWPFPicture 图片
 - XWPFTable 表格

### 表格文件处理
- xls/xlsx （Excel）
- CSV文件
- xlsx 是以XML为标准
- 数据
　- sheet 行 列 单元格

- 常见功能：解析 生成
- 第三方的包
 - **POI** JXL (免费)
 - COM4J (Windows平台)
 - Aspose等 (收费)

- 主要类 **xls/xlsx**
 - XSSFWorkbook 整个文档对象
 - XSSFSheet 整个sheet对象
 - XSSFRow 一行对象
 - XSSFCell 一个单元格

- CSV (逗号分隔)
 - 第三方包
 - Apache Commons CSV
 - CSVFormer 文档格式
 - CSVParser 解析文档
 - CSVRecord 一行记录
 - CSVPrinter 写入文档

### PDF简介及解析 
- 意为“便携式文档格式”
- PostScript 描述所有图形 保证在不同的机器上的颜色和打印效果
- 字型嵌入系统 可使字型随文件一起传输
- 结构化的存储系统 绑定元素和任何相关内容到单个文件 带有适当的数据压缩系统
- 常见功能处理 解析 生成
- 第三方包
 - (Apache PDFBox) 免费
 - iText 收费
 - XDocReport docx to pdf

- Apache PDFBox
 - 纯Java类库
 - 主要功能：创建 提取文本 分割、合并、删除
 - 主要类
 1. PDDocument pdf文档对象
 2. PDFTextStripper pdf文本对象
 3. PDFMergerUtility 合并工具
	
- XDocReport
 - **将docx文档合并输出为其他数据格式(pdf/html/...)**
 - PdfConverter
 - 基于poi和iText完成
 - 直接编辑PDF比较难 可以先通过编辑docx文档再转化为PDF文档

```
import com.google.zxing.BarcodeFormat;
import com.google.zxing.MultiFormatWriter;
import com.google.zxing.WriterException;
import com.google.zxing.client.j2se.MatrixToImageWriter;
import com.google.zxing.common.BitMatrix;
import fr.opensagres.poi.xwpf.converter.pdf.PdfConverter;
import fr.opensagres.poi.xwpf.converter.pdf.PdfOptions;
import org.apache.poi.openxml4j.exceptions.InvalidFormatException;
import org.apache.poi.util.Units;
import org.apache.poi.xssf.usermodel.XSSFCell;
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.apache.poi.xwpf.usermodel.*;


import javax.imageio.ImageIO;
import java.io.*;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

class Main {

    private static ArrayList<Person> personList, resultPersonList;
    private static Person person;
    private static File barCodeFile;
    private static XWPFDocument document;


    public static void main(String[] args) throws IOException, WriterException, InvalidFormatException {
        resultPersonList = readExcel("student.xlsx");
        for (Person p : resultPersonList) {
            System.out.println(p);
        }

        barCodeFile = new File("barCode.png");
        generateBarCode(barCodeFile, person.getCode(), 500, 250);
        modify();
        ConvertDocxToPdf();
    }
    
    public static void ConvertDocxToPdf(){
        String docxPath = "student1.docx";
        String pdfPath = "student.pdf";
        XWPFDocument document = null;
        try (InputStream doc = Files.newInputStream(Paths.get(docxPath))) {
            document = new XWPFDocument(doc);
        } catch (IOException e) {
            e.printStackTrace();
        }
        fr.opensagres.poi.xwpf.converter.pdf.PdfOptions options = PdfOptions.create();
        try (OutputStream out = Files.newOutputStream(Paths.get(pdfPath))) {
            PdfConverter.getInstance().convert(document, out, options);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void generateBarCode(File file, String code, int width, int height) throws WriterException, FileNotFoundException {
        BitMatrix matrix = null;
        MultiFormatWriter writer = new MultiFormatWriter();
        matrix = writer.encode(code, BarcodeFormat.CODE_128, width, height, null);
        FileOutputStream fileOutputStream = new FileOutputStream(file);
        try {
            ImageIO.write(MatrixToImageWriter.toBufferedImage(matrix), "png", fileOutputStream);
            fileOutputStream.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void writeBarCode(String imagePath) throws IOException, InvalidFormatException {
        XWPFParagraph xwpfParagraph = document.createParagraph();
        XWPFRun run = xwpfParagraph.createRun();
        run.addPicture(new FileInputStream(imagePath), XWPFDocument.PICTURE_TYPE_PNG, imagePath, Units.toEMU(200), Units.toEMU(40));
        FileOutputStream fileOutputStream = new FileOutputStream("student1.docx");
        document.write(fileOutputStream);


    }

    public static void modify() throws IOException, InvalidFormatException {
        InputStream is = new FileInputStream("student.docx");
        document = new XWPFDocument(is);
        List<IBodyElement> iBodyElements = document.getBodyElements();

        for (int i = iBodyElements.size()-1; i >=0 ; i--) {
            BodyElementType bodyElementType = iBodyElements.get(i).getElementType();
            if (bodyElementType == BodyElementType.TABLE) {
                XWPFTable table = (XWPFTable) iBodyElements.get(i);
                List<XWPFTableRow> tableRows = table.getRows();
                for (XWPFTableRow row : tableRows) {
                    List<XWPFTableCell> cells = row.getTableCells();
                    int studentInfoCnt = 0;
                    for (int k = 0; k < cells.size(); k++) {
                        if (cells.get(k).getText().equals("{name}")) {
                            //remove the text in the cell which already existed
                            cells.get(k).removeParagraph(0);
                            cells.get(k).setText(resultPersonList.get(studentInfoCnt).getName());
                        } else if (cells.get(k).getText().equals("{sex}")) {
                            //already removed the name cell, so the index of this cell would be previous index
                            cells.get(k).removeParagraph(0);
                            cells.get(k).setText(resultPersonList.get(studentInfoCnt++).getGender());
                        }
                    }
                }
            }
            if (bodyElementType != BodyElementType.TABLE) {
                XWPFParagraph xwpfParagraph = (XWPFParagraph) iBodyElements.get(i);
                if (xwpfParagraph.getText().equals("{barcode}")) {
                    List<XWPFRun> runs = xwpfParagraph.getRuns();
                    for (int j = runs.size()-1; j >=0; j--) {
                        xwpfParagraph.removeRun(j);
                    }
                    writeBarCode("barCode.png");
                }
            }
        }


        FileOutputStream fileOutputStream=new FileOutputStream("student1.docx");
        document.write(fileOutputStream);
        fileOutputStream.close();

    }


    public static ArrayList<Person> readExcel(String filePath) throws IOException {
        personList = new ArrayList<>();
        InputStream ExcelFileToRead = new FileInputStream(filePath);
        XSSFWorkbook wb = new XSSFWorkbook(ExcelFileToRead);

        XSSFSheet sheet = wb.getSheetAt(0);
        XSSFRow row;
        XSSFCell cell;

        Iterator rows = sheet.rowIterator();
        while (rows.hasNext()) {
            row = (XSSFRow) rows.next();
            Iterator cells = row.cellIterator();
            person = new Person();
            int position = 0;
            while (cells.hasNext()) {
                cell = (XSSFCell) cells.next();
                switch (position) {
                    case 0:
                        person.setName(cell.getStringCellValue());
                        break;
                    case 1:
                        person.setGender(cell.getStringCellValue());
                        break;
                    case 2:
                        person.setCode(cell.getStringCellValue());
                        break;
                    default:
                }
                position++;
            }
            personList.add(person);
        }
        return personList;
    }
}

```
#### 其中的docx转pdf踩坑在我的csdn上有记录[点击跳转](https://blog.csdn.net/weixin_45433059/article/details/117162052)