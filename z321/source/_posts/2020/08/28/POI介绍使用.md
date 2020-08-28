---
title: POI介绍使用
date: 2020-08-28 13:57:33
tags: POIs使用
categories: POI
description: POI是java一个操作文档的一个工具，常用于操作excel
---

# POI使用介绍

## POI是什么

  **(1)Apache POI是[Apache软件基金会](https://baike.baidu.com/item/Apache软件基金会)的开放源码函式库，POI提供API给Java程序对Microsoft Office格式档案读和写的功能 **

**(2)POI结构说明**

> **包名称	说明**

> HSSF	提供读写Microsoft Excel XLS格式档案的功能。

> XSSF	提供读写Microsoft Excel OOXML XLSX格式档案的功能。

> HWPF	提供读写Microsoft Word DOC格式档案的功能。

> HSLF	提供读写Microsoft PowerPoint格式档案的功能。

> HDGF	提供读Microsoft Visio格式档案的功能。

> HPBF	提供读Microsoft Publisher格式档案的功能。

> HSMF	提供读Microsoft Outlook格式档案的功能。

**(3)POI常用类说明**

> **类名**        **说明**

> HSSFWorkbook    Excel的文档对象

> HSSFSheet	  Excel的表单

> HSSFRow	      Excel的行

> HSSFCell	  Excel的格子单元

> HSSFFont      Excel字体

> HSSFDataFormat   格子单元的日期格式

> HSSFHeader     Excel文档Sheet的页眉

> HSSFFooter     Excel文档Sheet的页脚

> HSSFCellStyle    格子单元样式

> HSSFDateUtil    日期

> HSSFPrintSetup   打印

> HSSFErrorConstants  错误信息表

---

## POI常用操作

**（1）生成excel文件**

**实际工作中整理的代码:**  

  **核心操作：**

> 1.生成HSSFWorkbook对象用来操作excel
>
> HSSFWorkbook workbook = new HSSFWorkbook();

> 2.生成页面对象，设置excel标题
>
> HSSFSheet hssfSheet = workbook.createSheet("sheet1"); 

> 3.生成单元格样式对象，用来格式化单元格样式
>
> HSSFCellStyle titleCellStyle = workbook.createCellStyle();
>
> hssfCellStyle.setWrapText(true);//设置单元格自动换行

> 4.创建行对象
>
> HSSFRow row = hssfSheet.createRow(行索引);

> 5.创建单元格对象
>
> HSSFCell cell1 = row.createCell(单元格索引);

> 6.合并单元格表格
>
> hssfSheet.addMergedRegion(new CellRangeAddress(起始行, 终止行, 起始列,终止列) );



```java
	private void workWithJExcel(HttpServletRequest request,List<PadResult> resultList){
	OutputStream out =null; 
	Long fileName= resultList.get(0).getTaskId();//设置一个文件名称

	try {
	File file1 = new File(request.getServletContext().getRealPath("/upload2"));
	if (!file1.exists()) { //如果文件夹不存在生成文件夹
		file1.mkdirs();
	}
	File file2 =new File(file1, fileName+".xls");
	out =new FileOutputStream(file2); 
	HSSFWorkbook workbook = new HSSFWorkbook(); //第一步得到对象HSSFWorkbook HSSFWorkbook
	HSSFSheet hssfSheet = workbook.createSheet("sheet1"); //第二步创建页面对象
	hssfSheet.setColumnWidth(0, 30*256);//设置第一列宽度
	hssfSheet.setColumnWidth(1, 80*256);//设置第二列宽度
	hssfSheet.setColumnWidth(2, 15*256);//设置第三列宽度
	
	//如果生成的文件夹中有多个样式建议分开定制样式
        
    //标题的样式
	HSSFCellStyle titleCellStyle = workbook.createCellStyle(); //第三步步创建单元格样式
	titleCellStyle.setAlignment(HSSFCellStyle.ALIGN_CENTER);//水平居中 
	titleCellStyle.setVerticalAlignment(HSSFCellStyle.VERTICAL_CENTER);//垂直居中
    //第一列的样式
	HSSFCellStyle oneCellStyle = workbook.createCellStyle(); //第三步创建单元格样式
	oneCellStyle.setAlignment(HSSFCellStyle.ALIGN_CENTER);//水平居中 
	oneCellStyle.setVerticalAlignment(HSSFCellStyle.VERTICAL_CENTER);//垂直居中
	oneCellStyle.setBorderBottom(HSSFCellStyle.BORDER_THIN); //下边框
	oneCellStyle.setBorderLeft(HSSFCellStyle.BORDER_THIN);//左边框
	oneCellStyle.setBorderTop(HSSFCellStyle.BORDER_THIN);//上边框
	oneCellStyle.setBorderRight(HSSFCellStyle.BORDER_THIN);//右边框
	
	//第二列整体的样式
	HSSFCellStyle hssfCellStyle = workbook.createCellStyle(); //第三步创建单元格样式
	hssfCellStyle.setWrapText(true);     //自动换行 
	hssfCellStyle.setBorderBottom(HSSFCellStyle.BORDER_THIN); //下边框
	hssfCellStyle.setBorderLeft(HSSFCellStyle.BORDER_THIN);//左边框
	hssfCellStyle.setBorderTop(HSSFCellStyle.BORDER_THIN);//上边框
	hssfCellStyle.setBorderRight(HSSFCellStyle.BORDER_THIN);//右边框
	
	//第三列样式 字体红色
	HSSFCellStyle colorCellStyle = workbook.createCellStyle();
	colorCellStyle.setAlignment(HSSFCellStyle.ALIGN_CENTER);//水平居中 
	colorCellStyle.setVerticalAlignment(HSSFCellStyle.VERTICAL_CENTER);//垂直居中
	colorCellStyle.setBorderBottom(HSSFCellStyle.BORDER_THIN); //下边框
	colorCellStyle.setBorderLeft(HSSFCellStyle.BORDER_THIN);//左边框
	colorCellStyle.setBorderTop(HSSFCellStyle.BORDER_THIN);//上边框
	colorCellStyle.setBorderRight(HSSFCellStyle.BORDER_THIN);//右边框
	HSSFFont font = workbook.createFont();
	font.setColor(Font.COLOR_RED);
    colorCellStyle.setFont(font);
    
    int rowIndex=0;//创建行的索引，这样当你每次创建一行数据之后 增加1这样可以按照顺序生成行
        
    List<String> eNames= new ArrayList<String>(); 
	for (int i=0 ; i<resultList.size();i++){
		HSSFRow row=null;

		if(rowIndex==0 || !eNames.contains(resultList.get(i).getDevName())) {
			if(rowIndex!=0) {
			row = hssfSheet.createRow(rowIndex); // 第四步，创建行对象 
			rowIndex++;//前进一步
			}
			eNames.add(resultList.get(i).getDevName());
			row = hssfSheet.createRow(rowIndex); // 第四步，创建行对象 
	
		HSSFCell title1 = row.createCell(0);//创建标题这里之所以选择第0个单元格是要合并单元格的
			title1.setCellValue(resultList.get(i).getDevTypeName()+">>"+resultList.get(i).getDevName()); //设置标题
			title1.setCellStyle(titleCellStyle);//设置单元格样式
 //合并单元格表格hssfSheet.addMergedRegion(new CellRangeAddress(起始行, 终止行, 起始列,终止列) );
			hssfSheet.addMergedRegion(new CellRangeAddress(rowIndex, rowIndex, 0, 2) );
			rowIndex++;//前进一步
			
		}
			    row = hssfSheet.createRow(rowIndex);//创建行对象
			    rowIndex++;
			    row.setHeightInPoints(25);//设置行高
			    
			      
				HSSFCell cell1 = row.createCell(0);//创建单元格
				cell1.setCellStyle(oneCellStyle);
				cell1.setCellValue(resultList.get(i).getPartName());
				
				
				HSSFCell cell2 = row.createCell(1);//创建单元格
				cell2.setCellStyle(hssfCellStyle);
				
				Long templateId = resultList.get(i).getTemplateId();
				//查询模板内容
				Template template =taskDao.findTemplateById(templateId);
				cell2.setCellValue(template.getContent());
				
				HSSFCell cell3 = row.createCell(2);//创建单元格
	
				if(!(resultList.get(i).getIsPass()==null)){	
					cell3.setCellStyle(colorCellStyle);
					if(resultList.get(i).getIsPass()==1)
					cell3.setCellValue("√");
					else if(resultList.get(i).getIsPass()==0)
					cell3.setCellValue("×");
				}else {
					cell3.setCellStyle(colorCellStyle);
					cell3.setCellValue("");
				}

	}	
	workbook.write(out);//生成excel
	} catch (Exception e) { 
	e.printStackTrace(); 
	}finally {
		if(out!=null) 
			try { 
				out.close(); 
			} catch(IOException e){ 
		e.printStackTrace();
		}
	}
}
```

**（2）导入excel数据** 

**核心操作：**

> 1.创建操作对象
>
> InputStream in  = excl.getInputStream();//获取输入流
>
> Workbook workbook = new XSSFWorkbook(in);//创建XLSX文件操作对象
>
> Workbook workbook = new HSSFWorkbook(in);//创建XLS文件操作对象

> 2.获取页面对象 一个页面相当于是由一个ROW集合组成
>
> Sheet sheet = workbook.getSheetAt(页面索引);

> 3.创建行对象
>
> Row row = sheet.getRow(行索引)；//下面例子使用的是迭代器但是我们也可以使用for循环

> 4.获取单元格
>
> Cell cell = row.getCell(3).getStringCellValue(); //我们可以根据cell的类型载通过get得到不同的值类型

**<font color='red'>注意：事实上在获取单元格时应该判断这个单元格类型，根据单元格类型获取不同的Java类型的数据</font>**

```java
	/**
	 * 导入excl表格
	 */
	@Override
	public void importExclStationData(MultipartFile excl)throws FileTypeException, ParseException {
		List<String> names =findStatinNames();
	// 用来标记状态如果stations里面一个对象，也就是说一个对象都没有添加进去那么这个值为false 否则为true
		// boolean flag = false;
		InputStream in = null;
		try {
			in = excl.getInputStream();
			// 获取execl文件对象
			Workbook workbook = null;
			// 根据后缀，得到不同的Workbook子类，即HSSFWorkbook或XSSFWorkbook
			if (excl.getOriginalFilename().endsWith("xlsx")) {
				workbook = new XSSFWorkbook(in);//给定输入流读取文件创建XLSX操作对象
			} else if (excl.getOriginalFilename().endsWith("xls")) {
				workbook = new HSSFWorkbook(in);//给定输入流读取文件创建XLS操作对象
			} else {
				throw new FileTypeException();
			}

			// 获得sheet对应对象 获取第一页对象
			Sheet sheet = workbook.getSheetAt(0);
			// 创建Station对象容器
			List<Station> stations = new ArrayList<Station>();
			Station station;
			// 解析sheet,获得多行数据，并放入迭代器中
			Iterator<Row> ito = sheet.iterator();

			int count = 0;
			Row row = null;
			while (ito.hasNext()) {
				row = ito.next();
				// 由于第一行是标题因此这里单独处理
				if (count == 0) {
					++count;
					continue;
				} else {
					if (row != null) {// TODO
						//System.out.println(row.getCell(3).getStringCellValue().trim().equals(""));
						String name = row.getCell(3).getStringCellValue();
						if (!names.contains(name) && !(name.trim().equals(""))) {
							// flag=true;
							station = new Station();
							names.add(name);
							station.setName(name);
							
							station.setPowerLevel(row.getCell(37).toString());
							
							
							station.setStatus("Actived");
							
						
							station.setCity(row.getCell(45).toString());
							
					
							// 这个是设置创建人和最后一次修改的人
							station.setCreateBy(1L);
							station.setLastUpdateBy(1L);
							station.setCreateDate(getDate());
							station.setLastUpdateDate(getDate());
							station.setDesc("11111");
							stations.add(station);
						}
					}
				}
				++count;

			}
			// System.out.println(count);
			if (!(stations.size() <= 0)) {
				createStations(stations);
			}
			// if(flag&&row!=null) {
			// }
			importExclEquipmentDate(sheet, row);

		} catch (IOException e) {

			e.printStackTrace();
		} finally {
			if (in != null) {
				try {
					in.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
		
	}
```

**<font color='red'>附上：判断获取单元格类型的代码 可能代码有些许问题没有进行亲测但是这个判断条件应该加上去这样会提高代码健壮性</font>**

```java
public static Object getJavaValue(XSSFCell cell) {
         Object o = null;
         int cellType = cell.getCellType();
        switch (cellType) {
         case XSSFCell.CELL_TYPE_BLANK:
             o = "";
             break;
         case XSSFCell.CELL_TYPE_BOOLEAN:
             o = cell.getBooleanCellValue();
            break;
         case XSSFCell.CELL_TYPE_ERROR:
             o = "Bad value!";
             break;
         case XSSFCell.CELL_TYPE_NUMERIC:
             o = getValueOfNumericCell(cell);
             break;
         case XSSFCell.CELL_TYPE_FORMULA:
             try {
                 o = getValueOfNumericCell(cell);
             } catch (IllegalStateException e) {
                try {
                     o = cell.getRichStringCellValue().toString();
                 } catch (IllegalStateException e2) {
                     o = cell.getErrorCellString();
                 }
             } catch (Exception e) {
                 e.printStackTrace();
             }
             break;
         default:
             o = cell.getRichStringCellValue().getString();
         }
         return o;
     }
```

