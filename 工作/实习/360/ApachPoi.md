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

### 使用RegionUtil类为合并后的单元格添加边框
RegionUtil.setBorderBottom(org.apache.poi.ss.usermodel.BorderStyle.THIN, cellRangeAddress, sheet);