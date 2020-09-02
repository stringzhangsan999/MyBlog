---
title: 关于springmvc文件上传下载
date: 2020-09-02 09:19:02
tags: springmvc上传下载
categories: springframework之springmvc
description: 关于springmvc框架对上传下载的支持
---

# SpringMVC关于上传下载

## springmvc之上传文件

​    **任何文件想要上传到服务端，我们必须先给定一个文件上传的控件，在文件上传控件所在的表单之中我们前端准备工作**

**1.<font color='red'>method设置为post</font>**

**2.将表单的enctype设置<font color='red'>为multipart/form-data</font> 多部件表单类型**

**3.编写后端接口:**  **<font color='red'>System.getProperty("user.dir")获取系统文件路径</font>**

**文件上传后端编写的步骤：**

+ **获取文件名称并修改文件名称**
+ **设置文件上传路径**
+ **将文件使用流给上传上去     <font color='red'> 注意:流的上传有多种方式本质就是 ：获取源文件读取的流对象再设置一个文件写的流对象将文件写到另一个文件中</font>**

```java
public void fileUpLoad(MultipartFile myfile, HttpServletRequest request) throws FileTypeException {
		if (!myfile.isEmpty()) {
			String realPath = System.getProperty("user.dir") + "/src/main/resources/upload";//这里因为我使用的springboot内嵌的tomcat又因为运行时目录是动态的所以使用这种方式来得到项目文件的绝对路径

			// 获取上传文件名
			String oname = myfile.getOriginalFilename();

			if (oname.endsWith(".xlsx") || oname.endsWith(".xls")) {
				// 创建文件上传路径
				File file1 = new File(realPath);

				if (!file1.exists()) {
					file1.mkdirs();
				}

				File filePath = new File(realPath, oname);
				if (!filePath.getParentFile().exists()) {
					filePath.getParentFile().mkdirs();
				}
				// 防止文件重名问题
				String format = new SimpleDateFormat("yyyyMMddHHmmssSS").format(new Date());
				// System.out.println(File.separator);// "\"

				try {
					myfile.transferTo(new File(realPath + File.separator + format + "_" + oname));
				} catch (IllegalStateException e) {
					e.printStackTrace();
				} catch (IOException e) {
					e.printStackTrace();
				}

			} else {
				throw new FileTypeException();
			}

		}
	}
```



​    **代码解释：myfile.transferTo(new File(realPath + File.separator + format + "_" + oname));**

​    **这个是将文件拷贝到指定的目录下，使用的就是流的搬运，tranferTo(就是搬运到...)的意思 所以可以使用这个方式来实现文件上传的功能**



---

## springmvc 文件下载

​    **关于文件下载逻辑一般都在后端，<font color='red'>一般我们会在数据库中创建一个字段保存文件的路径，而这里使用的是通过主键加文件名后缀获取唯一文件名称</font>**

文件下载的步骤：

+ 得到文件对象并获取文件流对象
+ 设置相应内容设置响应头
+ 得到相应的文件输出对象
+ 将文件写入相应输出对象里面

```java
	public void downFile(String fileName, HttpServletRequest request , HttpServletResponse response) {
		File file = new File (request.getServletContext().getRealPath("upload2\\"+fileName));//这里使用的是通过文件真实路径加文件名称来获取文件对象，再获取流对象
		
		if (file == null || !file.exists()) {
            return;
        }
        OutputStream out = null;
        try {
            response.reset();//①清除首部的空白行。
            response.setContentType("application/octet-stream; charset=utf-8"); //设置相应内容及编码格式
            response.setHeader("Content-Disposition", "attachment; filename=" + file.getName());//设置响应头
            out = response.getOutputStream();//得到响应的文件输出流
            out.write(FileUtils.readFileToByteArray(file));//②
            out.flush();//刷新流对象，相当于把数据从缓存区中全部给刷新出来
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (out != null) {
                try {
                    out.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        } 
		
	}
```

**代码解释：①response.reset();reset的意思就是重置的意思将相应对象里面重置以下避免里面设置了其他东西或者是多余的东西**  

**②FileUtils.readFileToByteArray(file)将文件以字节的方式给读出来**

**③ out.write(FileUtils.readFileToByteArray(file)）将读到的东西写到页面**

**<font color='red'>注意：这里设置相应内容和响应头特别重要这是让浏览器知道我们这个是一个下载的操作：    response.setContentType("application/octet-stream; charset=utf-8"); //设置相应内容及编码格式
            response.setHeader("Content-Disposition", "attachment; filename=" + file.getName());//设置响应头</font>**



