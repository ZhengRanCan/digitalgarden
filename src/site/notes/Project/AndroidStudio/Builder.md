---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Builder/","dgPassFrontmatter":true}
---

`builder` 是一个用于构建对话框（Dialog）的辅助类。在 Android 中，如果你需要创建一个对话框，可以使用 `AlertDialog.Builder` 类来构建。它允许你通过链式调用方法来设置对话框的各种属性和按钮。
通过 `AlertDialog.Builder` 创建一个基本的对话框：
```java
AlertDialog.Builder builder = new AlertDialog.Builder(this);
builder.setTitle("标题");
builder.setMessage("消息内容");
builder.setPoistiveButton("确定",new DialogInterface.OnClickListener(){
	pubulic void onClick(DialogInterface dialog,int which){
		//点击按钮的逻辑
	}
});
```

权限确认对话框：
```java
private void showPermissionSettingsDialog() {  
	AlertDialog.Builder builder = new AlertDialog.Builder(this);  
	builder.setMessage("您已经拒绝了相机权限，您可以在应用设置中手动开启权限。");  
	builder.setPositiveButton("去设置", new DialogInterface.OnClickListener() {  
		@Override  
		public void onClick(DialogInterface dialog, int which) {  
		//dialog是对话框对象，which表示用户点击的按钮
		Intent intent = new Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS);  
		Uri uri = Uri.fromParts("package", getPackageName(), null);  
		//第一个参数用于指向应用程序的包名，第二个参数是一个Context类中的方法，用于获取当前应用程序的包名
		intent.setData(uri);  
		startActivity(intent);  
		}  
	});  
	builder.setNegativeButton("取消", null);  
	builder.show();  
}
```