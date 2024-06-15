---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/CameraX/","dgPassFrontmatter":true}
---

CameraX是一个Jetpack库，旨在帮助简化相机应用程序的开发
Camerax用例：目前来看，主要需要第一个和第三个用例
1. Image Capture
2. Video Capture
3. Preview
4. Image analyze

# 使用例
## 添加camerax依赖
```kotlin
// CameraX
cameraxVersion = '1.2.2'
implementation("androidx.camera:camera-lifecycle:$cameraxVersion") 
implementation("androidx.camera:camera-video:$cameraxVersion") 
implementation("androidx.camera:camera-view:$cameraxVersion") 
implementation("androidx.camera:camera-extensions:$cameraxVersion") 

// Accompanist
accompanistPermissionsVersion = '0.23.1'
implementation "com.google.accompanist:accompanist-permissions:$accompanistPermissionsVersion"

```

## 申请摄像头和音频权限
```kotlin
private lateinit var requestPermissionLauncher: ActivityResultLauncher<String>
override fun onCreate(savedInstanceState: Bundle?) {  
	super.onCreate(savedInstanceState)  
	  
	//权限申请  
	requestPermissionLauncher = registerForActivityResult(  
		ActivityResultContracts.RequestPermission()  
		) { isGranted: Boolean ->  
		if (isGranted) {  
			// 权限已授予，可以继续执行需要该权限的操作。
			setContent(){
				
			}  
		} else {  
		// 权限被拒绝，显示对话框指示用户跳转到设置页面手动授予权限  
			showPermissionDeniedDialog()  
		}  
	}
}
//设置弹窗，如果用户拒绝结束应用，如果不拒绝就跳转到权限界面  
private fun showPermissionDeniedDialog() {  
	AlertDialog.Builder(this)  
		.setTitle("权限被拒绝")  
		.setMessage("应用需要相机权限才能正常工作，请在设置中授予相机权限。")  
		.setPositiveButton("去设置") { _ , _ ->  
			// 跳转到应用设置页面  
			val intent = Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS).apply {  
			data = Uri.fromParts("package", packageName, null)  
			}  
		startActivity(intent)  
		}  
		.setNegativeButton("取消") { _, _ ->  
			// 关闭应用  
			finish()  
		}  
		.setOnDismissListener {  
			// 对话框被关闭时，关闭应用  
			finish()  
		}  
		.show()  
}
```

## 设置预览
```kotlin
private fun startCamera() {   
	//创建ProcessCameraProvider的实例
	//将相机的生命周期绑定到生命周期所有者，所以就不用就手动关闭相机了，因为现在绑定了
	val cameraProviderFuture = ProcessCameraProvider.getInstance(this)
	//添加监听器，添加一个runnable作为参数，然后再添加一个在主线程上运行的Executor
	cameraProviderFuture.addListener({       
		// 将相机的生命周期给绑定到应用进程中的lifecycleOwner
		val cameraProvider: ProcessCameraProvider = cameraProviderFuture.get()       
		// 初始化Preview对象，在其上调用build
		//获取Surface提供程序，然后在预览上进行设置
		val preview = Preview.Builder()          
				.build()          
				.also {              
					it.setSurfaceProvider(viewBinding.viewFinder.surfaceProvider) 
			    }       

		imageCapture = ImageCapture.Builder().build()
   
		// 创建CameraSelector对象，选择后摄     
		val cameraSelector = CameraSelector.DEFAULT_BACK_CAMERA      
		 try {           
			 // Unbind use cases before rebinding           
			 cameraProvider.unbindAll()           
			 // Bind use cases to camera           
			 cameraProvider.bindToLifecycle(               
				 this, cameraSelector, preview)       
		 } catch(exc: Exception) {          
			  Log.e(TAG, "Use case binding failed", exc)       
		  }   
	}, ContextCompat.getMainExecutor(this))
}
```
## 实现ImageCapture用例
```kotlin

private fun takePhoto() {   
	// 获取对ImageCapture用例的引用，  
	val imageCapture = imageCapture ?: return   
	// 创建用于保存图片的 MediaStore 内容值   
	// 使用时间戳，确保 MediaStore 中的显示名是唯一的
	val name = SimpleDateFormat(FILENAME_FORMAT, Locale.US)              
			.format(System.currentTimeMillis())   
	val contentValues = ContentValues().apply {       
		put(MediaStore.MediaColumns.DISPLAY_NAME, name)       
		put(MediaStore.MediaColumns.MIME_TYPE, "image/jpeg")       
		if(Build.VERSION.SDK_INT > Build.VERSION_CODES.P) { 
		     put(MediaStore.Images.Media.RELATIVE_PATH, "Pictures/CameraX-Image")       
		     }   
		}   
	// 创建一个OutputFileOptions对象,可以指定所需的输出内容
	val outputOptions = ImageCapture.OutputFileOptions           
		.Builder(contentResolver,                    
		 MediaStore.Images.Media.EXTERNAL_CONTENT_URI, 
		 contentValues)           
		 .build()   
	// 对 `imageCapture` 对象调用 `takePicture()`。传入 `outputOptions`、执行器和保存图片时使用的回调。  
	imageCapture.takePicture(       
		outputOptions,       
		ContextCompat.getMainExecutor(this),
		object : ImageCapture.OnImageSavedCallback {           
			 override fun onError(exc: ImageCaptureException) {               
				 Log.e(TAG, "Photo capture failed: ${exc.message}", exc)           
			}           
			 override fun onImageSaved(output: ImageCapture.OutputFileResults){               
				 val msg = "Photo capture succeeded: ${output.savedUri}"               
				 Toast.makeText(baseContext, msg, Toast.LENGTH_SHORT).show()               
				 Log.d(TAG, msg)           
			}       
		}   
	)
}

```

# REF:
1. [使用 CameraX 在 Jetpack Compose 中构建相机 Android 应用程序_galaxy s23 cameraxjetpack-CSDN博客](https://blog.csdn.net/u011897062/article/details/130824467) （用例为视频播放）
2. [CameraX 使用入门  |  Android Developers (google.cn)](https://developer.android.google.cn/codelabs/camerax-getting-started?hl=zh-cn#0)
3. [[Project/AndroidStudio/Executor\|Executor]]