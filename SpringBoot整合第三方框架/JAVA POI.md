<a name="BSRXO"></a>
## 
<a name="YsTYi"></a>
## 基本概念
在 POI 中，**Workbook**代表着一个 Excel 文件（工作簿），**Sheet**代表着 Workbook 中的一个表格，**Row** 代表 Sheet 中的一行，而 **Cell** 代表着一个单元格。 **HSSFWorkbook**对应的就是一个 .xls 文件，兼容 Office97-2003 版本。 **XSSFWorkbook**对应的是一个 .xlsx 文件，兼容 Office2007 及以上版本。 在 HSSFWorkbook 中，Sheet接口 的实现类为 HSSFSheet，Row接口 的实现类为HSSFRow，Cell 接口的实现类为 HSSFCell。 XSSFWorkbook 中实现类的命名方式类似，在 Sheet、Row、Cell 前加 XSSF 前缀即可。
<a name="zDxtp"></a>
## 使用Apache POI 读写Excel 文件
Apache POI 针对读写Excel 文件提供了三个组件:

| HSSF | XSSF | SXSSF |
| --- | --- | --- |
| HSSF是POI项目对Excel '97（-2007）文件格式的纯Java实现 | XSSF是POI项目对Excel 2007 OOXML（.xlsx）文件格式的纯Java实现。

  | SXSSF是XSSF的API兼容流扩展，可用于必须生成非常大的电子表格且堆空间有限的情况

  |
| 处理*.xls 文件

  | 处理*.xlsx 文件

  | 处理超大的*xlsx 文件

 |


生成电子表格的另一种方法是通过Cocoon序列化器（但是您仍将间接使用HSSF）。使用Cocoon，您可以通过简单地应用样式表并指定序列化程序来序列化任何XML数据源（例如，可能是在SQL中输出的ESQL页面）。<br />从3.8-beta3开始，POI提供了基于XSSF的低内存占用的SXSSF API。<br />SXSSF是XSSF的API兼容流扩展，可用于必须生成非常大的电子表格且堆空间有限的情况。SXSSF通过限制对滑动窗口内的行的访问来实现其低内存占用，而XSSF允许对文档中的所有行进行访问。不再存在于窗口中的较旧的行由于被写入磁盘而变得不可访问.<br />在自动刷新模式下，可以指定访问窗口的大小，以在内存中保留一定数量的行。当达到该值时，创建额外的一行会导致索引最低的行从访问窗口中删除并写入磁盘。或者，可以将窗口大小设置为动态增长。可以根据需要通过显式调用flushRows（int keepRows）定期对其进行修剪。<br />由于实现的流性质，与XSSF相比存在以下限制：<br />在某个时间点只能访问有限数量的行。<br />不支持Sheet.clone()。<br />不支持公式评估<br />电子表格API功能摘要<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1677949871750-74a3bd09-d25a-4dd8-a65d-49483d7c6395.png#averageHue=%23dab46f&clientId=u41f33a3e-e832-4&from=paste&id=u94feb204&originHeight=261&originWidth=578&originalType=url&ratio=1&rotation=0&showTitle=false&size=21424&status=done&style=none&taskId=u81c5c6c9-e815-455a-93cb-39ae3c31b98&title=)
<a name="gsfQ6"></a>
###  创建一个WorkBook
创建一个*.xls 文件
```java
Workbook wb = new HSSFWorkbook();
try  (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}

```
创建一个*.xlsx 文件
```java
Workbook wb = new XSSFWorkbook();
try (OutputStream fileOut = new FileOutputStream("workbook.xlsx")) {
    wb.write(fileOut);
}

```
<a name="StZJW"></a>
### 创建一个Sheet
```java
Workbook wb = new HSSFWorkbook();  // or new XSSFWorkbook();
Sheet sheet1 = wb.createSheet("new sheet");
Sheet sheet2 = wb.createSheet("second sheet");
// Note that sheet name is Excel must not exceed 31 characters
// and must not contain any of the any of the following characters:
// 0x0000
// 0x0003
// colon (:)
// backslash (\)
// asterisk (*)
// question mark (?)
// forward slash (/)
// opening square bracket ([)
// closing square bracket (])
// You can use org.apache.poi.ss.util.WorkbookUtil#createSafeSheetName(String nameProposal)}
// for a safe way to create valid names, this utility replaces invalid characters with a space (' ')
String safeName = WorkbookUtil.createSafeSheetName("[O'Brien's sales*?]"); // returns " O'Brien's sales   "
Sheet sheet3 = wb.createSheet(safeName);
try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}

```
<a name="DaVUJ"></a>
### 创建一个单元格
```java
Workbook wb = new HSSFWorkbook();
//Workbook wb = new XSSFWorkbook();
CreationHelper createHelper = wb.getCreationHelper();
Sheet sheet = wb.createSheet("new sheet");
// Create a row and put some cells in it. Rows are 0 based.
Row row = sheet.createRow(0);
// Create a cell and put a value in it.
Cell cell = row.createCell(0);
cell.setCellValue(1);
// Or do it on one line.
row.createCell(1).setCellValue(1.2);
row.createCell(2).setCellValue(
    createHelper.createRichTextString("This is a string"));
row.createCell(3).setCellValue(true);
// Write the output to a file
try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}

```
<a name="Ega8g"></a>
### 创建一个日期类型的单元格
```java
Workbook wb = new HSSFWorkbook();
//Workbook wb = new XSSFWorkbook();
CreationHelper createHelper = wb.getCreationHelper();
Sheet sheet = wb.createSheet("new sheet");
// Create a row and put some cells in it. Rows are 0 based.
Row row = sheet.createRow(0);
// Create a cell and put a date value in it.  The first cell is not styled
// as a date.
Cell cell = row.createCell(0);
cell.setCellValue(new Date());
// we style the second cell as a date (and time).  It is important to
// create a new cell style from the workbook otherwise you can end up
// modifying the built in style and effecting not only this cell but other cells.
CellStyle cellStyle = wb.createCellStyle();
cellStyle.setDataFormat(
    createHelper.createDataFormat().getFormat("m/d/yy h:mm"));
cell = row.createCell(1);
cell.setCellValue(new Date());
cell.setCellStyle(cellStyle);
//you can also set date as java.util.Calendar
cell = row.createCell(2);
cell.setCellValue(Calendar.getInstance());
cell.setCellStyle(cellStyle);
// Write the output to a file
try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}

```
<a name="CuDoe"></a>
### 创建多种格式的单元格
```java
Workbook wb = new HSSFWorkbook();
Sheet sheet = wb.createSheet("new sheet");
Row row = sheet.createRow(2);
row.createCell(0).setCellValue(1.1);
row.createCell(1).setCellValue(new Date());
row.createCell(2).setCellValue(Calendar.getInstance());
row.createCell(3).setCellValue("a string");
row.createCell(4).setCellValue(true);
row.createCell(5).setCellType(CellType.ERROR);
// Write the output to a file
try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}
```
<a name="yzjMC"></a>
### Files 和InputStream
当打开 WorkBook（.xls HSSFWorkbook或.xlsx XSSFWorkbook）时，可以从File 或InputStream加载工作簿。<br />使用File对象可以减少内存消耗，而InputStream需要更多内存，因为它必须缓冲整个文件。<br />如果使用WorkbookFactory，则使用其中一个非常容易：
```java
// Use a file
Workbook wb = WorkbookFactory.create(new File("MyExcel.xls"));
// Use an InputStream, needs more memory
Workbook wb = WorkbookFactory.create(new FileInputStream("MyExcel.xlsx"));

```
如果直接使用HSSFWorkbook或XSSFWorkbook，通常应遍历POIFSFileSystem或 OPCPackage，以完全控制生命周期（包括完成后关闭文件）：
```java
// HSSFWorkbook, File
POIFSFileSystem fs = new POIFSFileSystem(new File("file.xls"));
HSSFWorkbook wb = new HSSFWorkbook(fs.getRoot(), true);
....
fs.close();
// HSSFWorkbook, InputStream, needs more memory
POIFSFileSystem fs = new POIFSFileSystem(myInputStream);
HSSFWorkbook wb = new HSSFWorkbook(fs.getRoot(), true);
// XSSFWorkbook, File
OPCPackage pkg = OPCPackage.open(new File("file.xlsx"));
XSSFWorkbook wb = new XSSFWorkbook(pkg);
....
pkg.close();
// XSSFWorkbook, InputStream, needs more memory
OPCPackage pkg = OPCPackage.open(myInputStream);
XSSFWorkbook wb = new XSSFWorkbook(pkg);
....
pkg.close();

```
<a name="tlZN6"></a>
### 展示各种对齐方式
```java
public static void main(String[] args) throws Exception {
    Workbook wb = new XSSFWorkbook(); //or new HSSFWorkbook();
    Sheet sheet = wb.createSheet();
    Row row = sheet.createRow(2);
    row.setHeightInPoints(30);
    createCell(wb, row, 0, HorizontalAlignment.CENTER, VerticalAlignment.BOTTOM);
    createCell(wb, row, 1, HorizontalAlignment.CENTER_SELECTION, VerticalAlignment.BOTTOM);
    createCell(wb, row, 2, HorizontalAlignment.FILL, VerticalAlignment.CENTER);
    createCell(wb, row, 3, HorizontalAlignment.GENERAL, VerticalAlignment.CENTER);
    createCell(wb, row, 4, HorizontalAlignment.JUSTIFY, VerticalAlignment.JUSTIFY);
    createCell(wb, row, 5, HorizontalAlignment.LEFT, VerticalAlignment.TOP);
    createCell(wb, row, 6, HorizontalAlignment.RIGHT, VerticalAlignment.TOP);
    // Write the output to a file
    try (OutputStream fileOut = new FileOutputStream("xssf-align.xlsx")) {
        wb.write(fileOut);
    }
    wb.close();
}
/**
 * Creates a cell and aligns it a certain way.
 *
 * @param wb     the workbook
 * @param row    the row to create the cell in
 * @param column the column number to create the cell in
 * @param halign the horizontal alignment for the cell.
 * @param valign the vertical alignment for the cell.
 */
private static void createCell(Workbook wb, Row row, int column, HorizontalAlignment halign, VerticalAlignment valign) {
    Cell cell = row.createCell(column);
    cell.setCellValue("Align It");
    CellStyle cellStyle = wb.createCellStyle();
    cellStyle.setAlignment(halign);
    cellStyle.setVerticalAlignment(valign);
    cell.setCellStyle(cellStyle);
}

```
<a name="cfM6M"></a>
### 设置边框
```java
Workbook wb = new HSSFWorkbook();
Sheet sheet = wb.createSheet("new sheet");
// Create a row and put some cells in it. Rows are 0 based.
Row row = sheet.createRow(1);
// Create a cell and put a value in it.
Cell cell = row.createCell(1);
cell.setCellValue(4);
// Style the cell with borders all around.
CellStyle style = wb.createCellStyle();
style.setBorderBottom(BorderStyle.THIN);
style.setBottomBorderColor(IndexedColors.BLACK.getIndex());
style.setBorderLeft(BorderStyle.THIN);
style.setLeftBorderColor(IndexedColors.GREEN.getIndex());
style.setBorderRight(BorderStyle.THIN);
style.setRightBorderColor(IndexedColors.BLUE.getIndex());
style.setBorderTop(BorderStyle.MEDIUM_DASHED);
style.setTopBorderColor(IndexedColors.BLACK.getIndex());
cell.setCellStyle(style);
// Write the output to a file
try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}
wb.close()

```
<a name="V3ImB"></a>
### 遍历行和单元格
有时，可能只想遍历工作簿中的所有工作表中的所有行或行中的所有单元格。这可以通过简单的for循环来实现。

通过调用workbook.sheetIterator()， sheet.rowIterator()和row.cellIterator() 或隐式使用for-each循环，可以使用这些迭代器。请注意，rowIterator和cellIterator遍历已创建的行或单元格，跳过空的行和单元格。
```java
for (Sheet sheet : wb ) {
    for (Row row : sheet) {
        for (Cell cell : row) {
            // Do something here
        }
    }
}
```
<a name="Zw23x"></a>
### 遍历单元格，控制丢失/空白的单元格
在某些情况下，进行迭代时，您需要完全控制如何处理丢失或空白的行和单元格，并且需要确保访问每个单元格，而不仅仅是访问文件中定义的那些单元格。（CellIterator将仅返回文件中定义的单元格，这在很大程度上是具有值或样式的单元格，但取决于Excel）。

在这种情况下，应获取一行的第一列和最后一列信息，然后调用getCell（int，MissingCellPolicy） 来获取单元格。使用 MissingCellPolicy 控制空白或空单元格的处理方式。
```java
// Decide which rows to process
int rowStart = Math.min(15, sheet.getFirstRowNum());
int rowEnd = Math.max(1400, sheet.getLastRowNum());
for (int rowNum = rowStart; rowNum < rowEnd; rowNum++) {
   Row r = sheet.getRow(rowNum);
   if (r == null) {
      // This whole row is empty
      // Handle it as needed
      continue;
   }
   int lastColumn = Math.max(r.getLastCellNum(), MY_MINIMUM_COLUMN_COUNT);
   for (int cn = 0; cn < lastColumn; cn++) {
      Cell c = r.getCell(cn, Row.RETURN_BLANK_AS_NULL);
      if (c == null) {
         // The spreadsheet is empty in this cell
      } else {
         // Do something useful with the cell's contents
      }
   }
}
	
```
<a name="z2SUO"></a>
### 获取单元格内容
要获取单元格的内容，您首先需要知道它是哪种单元格（例如，将字符串单元格作为其数字内容将获得NumberFormatException）。因此，您将需要打开单元格的类型，然后为该单元格调用适当的getter。<br />在下面的代码中，我们遍历一张纸中的每个单元格，打印出该单元格的引用（例如A3），然后打印出该单元格的内容
```java
// import org.apache.poi.ss.usermodel.*;
DataFormatter formatter = new DataFormatter();
Sheet sheet1 = wb.getSheetAt(0);
for (Row row : sheet1) {
    for (Cell cell : row) {
        CellReference cellRef = new CellReference(row.getRowNum(), cell.getColumnIndex());
        System.out.print(cellRef.formatAsString());
        System.out.print(" - ");
        // get the text that appears in the cell by getting the cell value and applying any data formats (Date, 0.00, 1.23e9, $1.23, etc)
        String text = formatter.formatCellValue(cell);
        System.out.println(text);
        // Alternatively, get the value and format it yourself
        switch (cell.getCellType()) {
            case CellType.STRING:
                System.out.println(cell.getRichStringCellValue().getString());
                break;
            case CellType.NUMERIC:
                if (DateUtil.isCellDateFormatted(cell)) {
                    System.out.println(cell.getDateCellValue());
                } else {
                    System.out.println(cell.getNumericCellValue());
                }
                break;
            case CellType.BOOLEAN:
                System.out.println(cell.getBooleanCellValue());
                break;
            case CellType.FORMULA:
                System.out.println(cell.getCellFormula());
                break;
            case CellType.BLANK:
                System.out.println();
                break;
            default:
                System.out.println();
        }
    }
}

```
<a name="DVkaK"></a>
#### 文字提取
对于大多数文本提取要求，标准ExcelExtractor类应提供您所需要的全部。
```java
try (InputStream inp = new FileInputStream("workbook.xls")) {
    HSSFWorkbook wb = new HSSFWorkbook(new POIFSFileSystem(inp));
    ExcelExtractor extractor = new ExcelExtractor(wb);
    extractor.setFormulasNotResults(true);
    extractor.setIncludeSheetNames(false);
    String text = extractor.getText();
    wb.close();
}

```
:::success
对于非常精美的文本提取，XLS到CSV等，请查看 /src/examples/src/org/apache/poi/examples/hssf/eventusermodel/XLS2CSVmra.java
:::
<a name="WK4HM"></a>
#### 填充和颜色
```java
Workbook wb = new XSSFWorkbook();
Sheet sheet = wb.createSheet("new sheet");
// Create a row and put some cells in it. Rows are 0 based.
Row row = sheet.createRow(1);
// Aqua background
CellStyle style = wb.createCellStyle();
style.setFillBackgroundColor(IndexedColors.AQUA.getIndex());
style.setFillPattern(FillPatternType.BIG_SPOTS);
Cell cell = row.createCell(1);
cell.setCellValue("X");
cell.setCellStyle(style);
// Orange "foreground", foreground being the fill foreground not the font color.
style = wb.createCellStyle();
style.setFillForegroundColor(IndexedColors.ORANGE.getIndex());
style.setFillPattern(FillPatternType.SOLID_FOREGROUND);
cell = row.createCell(2);
cell.setCellValue("X");
cell.setCellStyle(style);
// Write the output to a file
try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}
wb.close();


```
<a name="k27U4"></a>
#### 合并单元格
```java
Workbook wb = new HSSFWorkbook();
Sheet sheet = wb.createSheet("new sheet");
Row row = sheet.createRow(1);
Cell cell = row.createCell(1);
cell.setCellValue("This is a test of merging");
sheet.addMergedRegion(new CellRangeAddress(
        1, //first row (0-based)
        1, //last row  (0-based)
        1, //first column (0-based)
        2  //last column  (0-based)
));
// Write the output to a file
try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}
wb.close();

```
<a name="OzFLi"></a>
#### 设置字体
```java
Workbook wb = new HSSFWorkbook();
Sheet sheet = wb.createSheet("new sheet");
// Create a row and put some cells in it. Rows are 0 based.
Row row = sheet.createRow(1);
// Create a new font and alter it.
Font font = wb.createFont();
font.setFontHeightInPoints((short)24);
font.setFontName("Courier New");
font.setItalic(true);
font.setStrikeout(true);
// Fonts are set into a style so create a new one to use.
CellStyle style = wb.createCellStyle();
style.setFont(font);
// Create a cell and put a value in it.
Cell cell = row.createCell(1);
cell.setCellValue("This is a test of fonts");
cell.setCellStyle(style);
// Write the output to a file
try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}
wb.close();


```
请注意，工作簿中唯一字体的最大数量限制为32767。您应该在应用程序中重新使用字体，而不是为每个单元格创建字体。

 例子：<br />~~错误的写法~~
```java
for (int i = 0; i < 10000; i++) {
    Row row = sheet.createRow(i);
    Cell cell = row.createCell(0);
    CellStyle style = workbook.createCellStyle();
    Font font = workbook.createFont();
    font.setBoldweight(Font.BOLDWEIGHT_BOLD);
    style.setFont(font);
    cell.setCellStyle(style);
}

```
正确的写法
```java
CellStyle style = workbook.createCellStyle();
Font font = workbook.createFont();
font.setBoldweight(Font.BOLDWEIGHT_BOLD);
style.setFont(font);
for (int i = 0; i < 10000; i++) {
    Row row = sheet.createRow(i);
    Cell cell = row.createCell(0);
    cell.setCellStyle(style);
}

```
<a name="kresK"></a>
#### 自定义颜色
HSSF（*.xls文件）
```java
HSSFWorkbook wb = new HSSFWorkbook();
HSSFSheet sheet = wb.createSheet();
HSSFRow row = sheet.createRow(0);
HSSFCell cell = row.createCell(0);
cell.setCellValue("Default Palette");
//apply some colors from the standard palette,
// as in the previous examples.
//we'll use red text on a lime background
HSSFCellStyle style = wb.createCellStyle();
style.setFillForegroundColor(HSSFColor.LIME.index);
style.setFillPattern(FillPatternType.SOLID_FOREGROUND);
HSSFFont font = wb.createFont();
font.setColor(HSSFColor.RED.index);
style.setFont(font);
cell.setCellStyle(style);
//save with the default palette
try (OutputStream out = new FileOutputStream("default_palette.xls")) {
    wb.write(out);
}
//now, let's replace RED and LIME in the palette
// with a more attractive combination
// (lovingly borrowed from freebsd.org)
cell.setCellValue("Modified Palette");
//creating a custom palette for the workbook
HSSFPalette palette = wb.getCustomPalette();
//replacing the standard red with freebsd.org red
palette.setColorAtIndex(HSSFColor.RED.index,
        (byte) 153,  //RGB red (0-255)
        (byte) 0,    //RGB green
        (byte) 0     //RGB blue
);
//replacing lime with freebsd.org gold
palette.setColorAtIndex(HSSFColor.LIME.index, (byte) 255, (byte) 204, (byte) 102);
//save with the modified palette
// note that wherever we have previously used RED or LIME, the
// new colors magically appear
try (out = new FileOutputStream("modified_palette.xls")) {
    wb.write(out);
}

```
XSSF(*.xlsx 文件)
```java
XSSFWorkbook wb = new XSSFWorkbook();
XSSFSheet sheet = wb.createSheet();
XSSFRow row = sheet.createRow(0);
XSSFCell cell = row.createCell( 0);
cell.setCellValue("custom XSSF colors");
XSSFCellStyle style1 = wb.createCellStyle();
style1.setFillForegroundColor(new XSSFColor(new java.awt.Color(128, 0, 128), new DefaultIndexedColorMap()));
style1.setFillPattern(FillPatternType.SOLID_FOREGROUND);

```
<a name="qByFr"></a>
#### 读和写WorksBooks
```java
try (InputStream inp = new FileInputStream("workbook.xls")) {
//InputStream inp = new FileInputStream("workbook.xlsx");
    Workbook wb = WorkbookFactory.create(inp);
    Sheet sheet = wb.getSheetAt(0);
    Row row = sheet.getRow(2);
    Cell cell = row.getCell(3);
    if (cell == null)
        cell = row.createCell(3);
    cell.setCellType(CellType.STRING);
    cell.setCellValue("a test");
    // Write the output to a file
    try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
        wb.write(fileOut);
    }
}

```
<a name="lYJAv"></a>
#### 在单元格中使用换行符
```java
Workbook wb = new XSSFWorkbook();   //or new HSSFWorkbook();
Sheet sheet = wb.createSheet();
Row row = sheet.createRow(2);
Cell cell = row.createCell(2);
cell.setCellValue("Use \n with word wrap on to create a new line");
//to enable newlines you need set a cell styles with wrap=true
CellStyle cs = wb.createCellStyle();
cs.setWrapText(true);
cell.setCellStyle(cs);
//increase row height to accommodate two lines of text
row.setHeightInPoints((2*sheet.getDefaultRowHeightInPoints()));
//adjust column width to fit the content
sheet.autoSizeColumn(2);
try (OutputStream fileOut = new FileOutputStream("ooxml-newlines.xlsx")) {
    wb.write(fileOut);
}
wb.close();

```
<a name="zysME"></a>
#### 数据格式化
```java
Workbook wb = new HSSFWorkbook();
Sheet sheet = wb.createSheet("format sheet");
CellStyle style;
DataFormat format = wb.createDataFormat();
Row row;
Cell cell;
int rowNum = 0;
int colNum = 0;
row = sheet.createRow(rowNum++);
cell = row.createCell(colNum);
cell.setCellValue(11111.25);
style = wb.createCellStyle();
style.setDataFormat(format.getFormat("0.0"));
cell.setCellStyle(style);
row = sheet.createRow(rowNum++);
cell = row.createCell(colNum);
cell.setCellValue(11111.25);
style = wb.createCellStyle();
style.setDataFormat(format.getFormat("#,##0.0000"));
cell.setCellStyle(style);
try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}
wb.close();

```
<a name="lDECl"></a>
#### 将工作表调整为一页
```java
Workbook wb = new HSSFWorkbook();
Sheet sheet = wb.createSheet("format sheet");
PrintSetup ps = sheet.getPrintSetup();
sheet.setAutobreaks(true);
ps.setFitHeight((short)1);
ps.setFitWidth((short)1);
// Create various cells and rows for spreadsheet.
try (OutputStream fileOut = new FileOutputStream("workbook.xls")) {
    wb.write(fileOut);
}
wb.close();

```
