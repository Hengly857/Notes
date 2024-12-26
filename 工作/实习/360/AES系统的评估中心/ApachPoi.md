# 三六零安全科技股份有限公司/数字安全集团/安全大脑事业部
360本地安全大脑
## 记录Apach POI开源组件开发笔记
```
配置文件修改
修改对应的服务 IP 地址
外部服务：
ElasticSearch 日志检索
MongoDB 内容包存储
kafka 许可证消息队列
nacos 本脑服务注册，用于HA

AES 其他服务组件
data_api 数据库访问接口
report_engine 报告引擎
phish_server 钓鱼邮件
cloud_config 云端攻击服务
local_cloud_config 本地攻击服务
```
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

### Linux环境下 如何稳定的监控指定进程的所有执行信息（包含子进程的、需要的权限资源越少越好）

### 基于现在的word文档生成pdf文档
方案1 itexpdf 生成 pdf（word生成是依靠 文本替换的占位符 生成）pdf希望依靠 基于表单字段创建的
结论：无法动态生成 pass

方案2 将使用FreeMarker作为模板引擎，并结合Flying Saucer（XHTMLRenderer）库来转换html文件生成PDF文件。
参考网站 https://blog.csdn.net/weixin_41701290/article/details/140714976
模板引擎是一种用于生成动态内容的类库（或框架），通过将预定义的模板与特定数据合并，来生成最终的输出。

首先就是提供现成的模板文件语法和解析能力。开发者只要按照特定要求去编写模板文件，
比如使用 ${参数} 语法，模板引擎就能自动将参数注入到模板中，得到完整文件，不用再自己编写解析逻辑了。
其次，模板引擎可以将数据和模板分离，让不同的开发人员独立工作。比如后端专心开发业务逻辑提供数据，前端专心写模板等，让系统更易于维护。
使用 知名的、稳定的经典模板引擎 FreeMarker。
FreeMarker 官方文档： https://freemarker.apache.org/docs/index.html

在html转换成pdf的过程需要考虑一下多种方案
1.Flying Saucer（XHTMLRenderer）库 似乎生成图片有问题
2.目前使用itexpdf {
    生成html无法显示图片 因为路径问题
    html转换成pdf 会导致页面空白
}
wkhtmltopdf
优点：
支持完整的 HTML5 和 CSS3
渲染效果最接近浏览器
中文支持很好