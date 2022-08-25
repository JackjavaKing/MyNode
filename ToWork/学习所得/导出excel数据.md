```java
public void exportExcel(HttpServletRequest request, HttpServletResponse response) throws IOException{
	/**  创建excel表格  */
	// 1.创建HSSFWorkbook对象(excel的文档对象)
	HSSFWorkbook wb = new HSSFWorkbook();
	// 2.建立新的sheet对象（excel的表单）
	HSSFSheet sheet = wb.createSheet("测试导出表格");
   
	sheet.setDefaultColumnWidth(20);         //设置缺省列宽
	sheet.setDefaultRowHeight((short)22);    // 设置缺省行高
	sheet.setDefaultRowHeightInPoints(22);
	
	/** 添加样式 */
	HSSFCellStyle cellDefStyle = wb.createCellStyle();   // 创建单元格样式
	cellDefStyle.setAlignment(HSSFCellStyle.ALIGN_CENTER);   // 设置左右居中
	cellDefStyle.setVerticalAlignment(HSSFCellStyle.VERTICAL_CENTER);    // 设置水平居中
	// 设置单元格边框（四周都有边框线）
	cellDefStyle.setBorderTop(HSSFCellStyle.BORDER_THIN);    // HSSFCellStyle.BORDER_NONE:无边框线
	cellDefStyle.setBorderBottom(HSSFCellStyle.BORDER_THIN);
	cellDefStyle.setBorderLeft(HSSFCellStyle.BORDER_THIN);
	cellDefStyle.setBorderRight(HSSFCellStyle.BORDER_THIN);
	cellDefStyle.setWrapText(true);  // 自动换行
	// 设置文字样式
	HSSFFont fontDef = wb.createFont();
	fontDef.setFontName("宋体");    // 设置字体类型
	fontDef.setFontHeightInPoints((short)12);    // 设置字体大小
	fontDef.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);   // 设置字体加粗（fontDef.setBold(true)同样有用）
	fontDef.setUnderline(HSSFFont.U_SINGLE);    // 设置下划线
	cellDefStyle.setFont(fontDef);
  
	
	/** 设置标题  */
	// 1.在sheet里创建第一行，参数为行索引(excel的行)，可以是0～65535之间的任何一个
	int rowIndex = 0;    // 记录行号
	HSSFRow titleRow = sheet.createRow(rowIndex);
	titleRow.setHeightInPoints(30);    // 设置此行的行高
	// 2.创建单元格（excel的单元格，参数为列索引，可以是0～255之间的任何一个
	HSSFCell cell = titleRow.createCell(0);
	cell.setCellValue("测试导出Excel表格");    // 设置单元格内容
	// 3.设置单元格样式
	cell.setCellStyle(cellDefStyle);
	// 4.合并单元格CellRangeAddress构造参数依次表示起始行，截至行，起始列， 截至列
	sheet.addMergedRegion(new CellRangeAddress(rowIndex, rowIndex, 0, 5));
	++ rowIndex;
	
	/** 设置表格内容  */
	// 1.获取要填入表格的数据(根据自己实际情况获取数据)
	List<T> datas = 。。。
	for (T obj : datas) {
		HSSFRow contentRow = sheet.createRow(rowIndex);
		cell = contentRow.createCell(0);
		cell.setCellValue(obj.getName());
		cell.setCellStyle(cellDefStyle);
		cell = contentRow.createCell(1);
		cell.setCellValue(obj.getSex());
		cell.setCellStyle(cellDefStyle);
		sheet.setColumnWidth(colIndex -1,  256 * 20);    // 设置列宽(第一个参数是列号, 第二个参数是宽度)(setColumnWidth这个方法宽度的单位是字符数的256分之一)
		cell = contentRow.createCell(2);
		cell.setCellValue(obj.getAge());
		cell.setCellStyle(cellcontentStyle);
		sheet.setColumnWidth(colIndex - 1,  256 * 80);
		sheet.addMergedRegion(new CellRangeAddress(rowIndex, rowIndex, 2, 5));    // 合并单元格CellRangeAddress构造参数依次表示起始行，截至行，起始列，截止列
		++ rowIndex;
	}
	
	
	/** 设置行高（前面设置了默认行高，这里只需设置非默认部分，） */
	for (int i = 2; i < rowIndex - 1; i++) {
		HSSFRow row = sheet.getRow(i);
		row.setHeightInPoints(22);
	}
	
	/** 画表格对角线(表格中常有需要画对角线) */
	HSSFPatriarch patriarch = sheet.createDrawingPatriarch();
	HSSFClientAnchor anchor = new HSSFClientAnchor();   // 建立锚点（startRow, startCol, startX, startY, endRow, endCol, endX, endY）
	anchor.setAnchor((short) 1, 5, 0, 0, (short) 3, 6, 0, 0);
	HSSFSimpleShape line = patriarch.createSimpleShape(anchor);    // 建立一个简单的图形
	line.setShapeType(HSSFSimpleShape.OBJECT_TYPE_LINE);    // 线
	line.setLineStyle(HSSFShape.LINESTYLE_SOLID);    // 实线
	line.setLineWidth(HSSFShape.LINEWIDTH_ONE_PT);   // 设置线条粗细(HSSFShape.LINEWIDTH_ONE_PT表示1pt)
	
	//输出Excel文件
	OutputStream output = response.getOutputStream();
	response.reset();
	response.setCharacterEncoding("UTF-8");        
	String fileName = "导出Excel表格测试文件.xls";
	response.setContentType("application/vnd.ms-excel");
	response.setHeader("Content-disposition", "attachment; filename=" + dealFileName(request, fileName) );
	wb.write(output);
	output.close();
}
 
/** 处理导出文件名乱码的问题 */ 
public String dealFileName(HttpServletRequest request, String fileName) {
	String returnFileName = "";
	try {
		String userAgent = request.getHeader("USER-AGENT");
		if (userAgent != null && (userAgent.indexOf("MSIE") != -1 || userAgent.indexOf("Trident") != -1) {    // IE浏览器
			returnFileName = java.net.URLEncoder.encode(fileName, "UTF8");
		} else if (userAgent != null && userAgent.indexOf("Mozilla") != -1) {    // 火狐,chrome浏览器等
			returnFileName = new String(fileName.getBytes("UTF-8"), "iso-8859-1");
		}
	} catch (Exception e) {
		e.printStackTrace();
	}
	return returnFileName;
}
```

