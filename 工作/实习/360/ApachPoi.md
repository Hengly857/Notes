# 记录Apach POI开源组件开发笔记

# 1. 命令行调用报告引擎，将上述参数传递保存在本地mongodb中，并且生成一个id,将id传递给报告引擎
# 2. 报告引擎带着上述id，调用 serverX的get_report_data的接口，拿到接口数据，可以将报告存放的路径也放在返回数据中
# 3. 报告引擎生成报告，并且报告生成结果给serverX

### 反射机制
    ExcelOwaspSheetDataSecurity currentSecurity = this.securityList.get(i);
    Class<?> clazz = currentSecurity.getClass();
    for (int j = 0; j < securityTitleKey.size(); j++) {
        try {
            String keyString = "";
            Object keyValue = null;
            String keyType = "";
            if (securityTitleKey.get(j).get(2).equals("Field")) {
                keyString = securityTitleKey.get(j).get(0);
                keyValue = clazz.getDeclaredField(keyString).get(currentSecurity);

数据处理好了 需要拿到 report_svc
get_report_data()->init_deal_device(self) 反序列化需要有数据
写一个schema反序列化出数据
ExcelReportDataSchema

owasp_type_sheet = ma.fields.Nested(
    ExcelOWASPStatisticSheetSchema(), data_key="owaspTypeSheet", metadata={"description": "OWASP类型统计"}
)
516里面有数据

### 设置条形图方向为垂直
CellRangeAddress(起始行号，终止行号， 起始列号，终止列号）

### 通过颜色的RGB值，可以在Java中使用Apache POI来设置相同的背景颜色
```
XSSFColor customColor = new XSSFColor(new byte[] {alpha, red, green, blue});
frameStyle.setFillForegroundColor(new XSSFColor(new byte[] {(byte)255, (byte)220, (byte)230, (byte)241}));
```
### 增加安全设备sheet表

### 根据模板生成的word目录自动更新
使用Apache POI生成Word文档时，会遇到目录（TOC, Table of Contents）需要手动更新的问题。这是因为Word中的目录是基于特定的样式（如Heading 1, Heading 2等）自动生成的，
而这些样式在文档中被标记为域代码。当文档内容发生变化时，这些域代码不会自动更新，因此需要用户手动点击“更新目录”按钮来刷新。
、、、
try (OPCPackage opcPackage = OPCPackage.open(outputFilename);
    XWPFDocument fs = new XWPFDocument(opcPackage)) {
 
    List<XWPFParagraph> paragraphs = fs.getParagraphs();
        for (XWPFParagraph paragraph : paragraphs) {
            if (paragraph.getCTP().getFldSimpleList() != null && !paragraph.getCTP().getFldSimpleList().isEmpty()) {
            for (org.openxmlformats.schemas.wordprocessingml.x2006.main.CTSimpleField field : paragraph.getCTP().getFldSimpleList()) {
                if ("TOC".equals(field.getInstr())) {
                    field.setDirty(true); // 设置为需要更新
                    log.debug("Marked TOC field as dirty");
                }
            }
    }
}
 
    // 强制更新整个文档的所有域
    fs.enforceUpdateFields();
、、、

### 适配邮件类型 对于邮件类型的excel增加收件箱

