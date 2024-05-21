---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Activity Result API/","dgPassFrontmatter":true}
---

1. 代码展示
```java
private ActivityResultLauncher<Intent> galleryLauncher;//定义载体


//申请
galleryLauncher = registerForActivityResult(new ActivityResultContracts.StartActivityForResult(), result -> {  
if (result.getResultCode() == RESULT_OK && result.getData() != null) {  
		//处理数据 
}  
});  

//发送intent
Intent intent = new Intent(Intent.ACTION_PICK);  
intent.setType("image/*");  
galleryLauncher.launch(intent);
});
```
2. registerForActivityResult：载体，定义协定，回调
	1. ActivityResultLauncher : 一个Activity启动器，作用是承载启动对象和返回对象，通过registerFroActivityResult返回该对象。
		1. registerForActivityResult () 方法的返回值是一个ActivityResultLauncher对象，这个对象当中有一个launch () 方法可以用于去启用Intent。这样我们就不需要再调用startActivityForResult () 方法了，而是直接调用launch () 方法，并把Intent传入即可。
	2. **ActivityResultContract** : 用来协定所需的输入类型以及结果的输出类型，
		1. Android默认提供了一些常用的定义，例如上面所使用到到ActivityResultContracts.StartActivityForResult()；
		2. 默认协定：
	3. ActivityResultCallback :
		1. 启动Activity并返回到当前Activity的结果回调。
<center>ActivityResultContract参数协定</center>

| ActivityResultContracts   | 说明                                          | 参数                   | 回调                                  |
| ------------------------- | ------------------------------------------- | -------------------- | ----------------------------------- |
| StartActivityForResult    | 差不多就是StartActivityForResult                 | Intent               | ActivityResult (code, data)         |
| TakePitcutre              | 通过MediaStore. ACTION_IMAGE_CAPTURE拍照并保存     | 保存文件的Uri             | 是否保存成功                              |
| TakePicutrePreview        | 通过MediaStore. ACTION_IMAGE_CAPTURE拍照        | null                 | 图片的Bitmap                           |
| CaputreVideo              | 通过MediaStore. ACTION_VIDEO_CAPTURE拍照拍摄视频并保存 | 保存文件的Uri             | 是否保存成功                              |
| RequestPermission<br>     | 请求单个权限                                      | Manifest. permission | 用户是否授予该权限                           |
| RequestMultiplePermission | 请求多个权限                                      |                      | 回调为map，key为请求的权限<br>value为用户是否授予该权限 |
|                           |                                             |                      |                                     |


3. registerForActivityResult () 可以多次调用以注册多个ActivityResultLauncher实例，用来处理不同Activity结果

## 注意点：
1. [[用户权限申请 \|用户权限申请 ]]
	1. 有Activity Result API后，不再需要在清单文件中明确声明“WRITE_EXTERNAL_STORE”等敏感权限，需要在代码中动态请求这些权限，并在用户同意后才能访问这些权限。
		1. 这是因为Andorid11中引入了一种叫做“[[Project/AndroidStudio/分区存储\|分区存储]]”（Scoped Storage）的新存储模型，它更严格地限制了应用对共享存储空间的直接访问。


# REF:
1. [startActivityForResult被标记为弃用后，如何优雅的启动Activity?_startactivityforresult弃用-CSDN博客](https://blog.csdn.net/chuyouyinghe/article/details/122577257?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-122577257-blog-130004237.235^v43^pc_blog_bottom_relevance_base7&spm=1001.2101.3001.4242.1&utm_relevant_index=3) 
2. [Activity Result API详解，是时候放弃startActivityForResult了_startactivityforresult 被废弃-CSDN博客](https://blog.csdn.net/guolin_blog/article/details/121063078)