---
{"tags":["Project/AndroidStudio","Project/省中医APP开发"],"dg-publish":true,"permalink":"/Project/省中医APP开发/使用opencv进行人脸检测/","dgPassFrontmatter":true}
---

## 代码片段 ：
```java
package com.example.myapplication.takephoto;  
  
import android.content.Context;  
import android.content.Intent;  
import android.graphics.Bitmap;  
import android.net.Uri;  
import android.os.Bundle;  
import android.provider.MediaStore;  
import android.util.Log;  
import android.widget.Button;  
import android.widget.ImageButton;  
import android.widget.Toast;  
  
import androidx.activity.result.ActivityResultLauncher;  
import androidx.activity.result.contract.ActivityResultContracts;  
import androidx.appcompat.app.AppCompatActivity;  
  
import com.example.myapplication.R;  
  
import org.opencv.android.OpenCVLoader;  
import org.opencv.android.Utils;  
import org.opencv.core.CvType;  
import org.opencv.core.Mat;  
import org.opencv.core.MatOfRect;  
import org.opencv.core.Point;  
import org.opencv.core.Rect;  
import org.opencv.core.Scalar;  
import org.opencv.core.Size;  
import org.opencv.imgproc.Imgproc;  
import org.opencv.objdetect.CascadeClassifier;  
  
import java.io.File;  
import java.io.FileOutputStream;  
import java.io.IOException;  
import java.io.InputStream;  
  
public class photo_album extends AppCompatActivity {  
  
//初始化控件  
ImageButton button;  
Button button1;  
Button button2;  
Button button3;  
Bitmap bitmap;  
  
//级联器  
CascadeClassifier mJavaDetector,mNoseDetector;  
float mRelativeFaceSize = 0.2f;  
int mAbsoluteFaceSize = 0;  
Mat mGray;  
File mCascadeFile;  
private static final String TAG = "OCVSample::Activity";  
private static final Scalar FACE_RECT_COLOR = new Scalar(0, 255, 0, 255);  
private ActivityResultLauncher<Intent> galleryLauncher;  
public static final int JAVA_DETECTOR = 0;  
  
@Override  
protected void onCreate(Bundle savedInstanceState) {  
super.onCreate(savedInstanceState);  
setContentView(R.layout.activity_photo_album);  
  
//加载opencv库  
OpenCVLoader.initLocal();  
  
  
  
//初始化控件  
button = findViewById(R.id.imageButton);  
button1 = findViewById(R.id.button8);  
button2 = findViewById(R.id.button7);  
button3 = findViewById(R.id.button6);  
  
//选取图片后，对图片进行处理  
galleryLauncher = registerForActivityResult(new ActivityResultContracts.StartActivityForResult(), result -> {  
if (result.getResultCode() == RESULT_OK && result.getData() != null) {  
final Uri imageUri =result.getData().getData();  
try {  
//获取相册图片  
bitmap = MediaStore.Images.Media.getBitmap(this.getContentResolver(),imageUri);  
convertGray();  
} catch (IOException e) {  
e.printStackTrace();  
Toast.makeText(photo_album.this, "出现错误", Toast.LENGTH_SHORT).show();  
}  
}  
});  
  
  
//打开相册  
button.setOnClickListener(v->openGallery());  
  
//跳转到相机页面  
button1.setOnClickListener(v->{  
Intent intent = new Intent(photo_album.this, Camera2ApiActivity.class);  
startActivity(intent);  
});  
  
//打开相册  
button2.setOnClickListener(v->openGallery());  
//跳转到报告部分  
// button3.setOnClickListener(v->{  
// Intent intent = new Intent(photo_album.this, Camera2ApiActivity.class);  
// startActivity(intent);  
// });  
  
}  
  
//打开相册  
private void openGallery() {  
Intent intent = new Intent(Intent.ACTION_PICK);  
intent.setType("image/*");  
galleryLauncher.launch(intent);  
}  
  
//图像处理  
private void convertGray(){  
  
Mat src = new Mat();  
Mat temp = new Mat();  
Mat dst = new Mat();  
// Convert Bitmap to Mat  
Utils.bitmapToMat(bitmap, src);  
  
//两次转化  
Imgproc.cvtColor(src, temp, Imgproc.COLOR_BGRA2BGR);  
Log.i("CV", "image type:" + (temp.type() == CvType.CV_8UC3));  
Imgproc.cvtColor(temp, dst, Imgproc.COLOR_BGR2GRAY);  
Utils.matToBitmap(dst, bitmap);  
mGray = dst;  
//加载级联器  
mJavaDetector = loadDetector(R.raw.lbpcascade_frontalface,"lbpcascade_frontalface.xml");  
mNoseDetector = loadDetector(R.raw.haarcascade_mcs_nose,"haarcascade_mcs_mouth.xml");  
//处理面部尺寸  
if (mAbsoluteFaceSize == 0) {  
int height = mGray.rows();  
if (Math.round(height * mRelativeFaceSize) > 0) {  
mAbsoluteFaceSize = Math.round(height * mRelativeFaceSize);  
}  
}  
  
// //检测面部  
MatOfRect faces = new MatOfRect();  
if (mJavaDetector != null) {  
mJavaDetector.detectMultiScale(mGray, faces, 1.1, 2, 2, // TODO: objdetect.CV_HAAR_SCALE_IMAGE  
new Size(300,300), new Size());  
}  
Rect[] facesArray = faces.toArray();  
for (int i = 0; i < facesArray.length; i++) {  
Imgproc.rectangle(src, facesArray[i].tl(), facesArray[i].br(), FACE_RECT_COLOR, 3);  
}  
for (int i = 0; i < facesArray.length; i++) {  
  
Log.e(TAG, "start to detect nose!");  
Mat faceROI = mGray.submat(facesArray[i]);  
MatOfRect noses = new MatOfRect();  
mNoseDetector.detectMultiScale(faceROI, noses, 1.1, 2, 2,  
new Size(100, 100));  
  
Rect[] nosesArray = noses.toArray();  
Imgproc.rectangle(src,  
new Point(facesArray[i].tl().x + nosesArray[0].tl().x, facesArray[i].tl().y + nosesArray[0].tl().y) ,  
new Point(facesArray[i].tl().x + nosesArray[0].br().x, facesArray[i].tl().y + nosesArray[0].br().y) ,  
FACE_RECT_COLOR, 3);  
  
Imgproc.rectangle(src, facesArray[i].tl(), facesArray[i].br(), FACE_RECT_COLOR, 3);  
}  
  
// 将opencv的Mat对象转换成Bitmap对象，并将其显示到Button对象中。  
Utils.matToBitmap(src, bitmap);  
button.setImageBitmap(bitmap);  
}  
  
//将预训练模型加载到手机上  
private CascadeClassifier loadDetector(int rawID,String fileName) {  
CascadeClassifier classifier = null;  
try {  
  
// load cascade file from application resources  
InputStream is = getResources().openRawResource(rawID);  
File cascadeDir = getDir("cascade", Context.MODE_PRIVATE);  
mCascadeFile = new File(cascadeDir, fileName);  
FileOutputStream os = new FileOutputStream(mCascadeFile);  
  
byte[] buffer = new byte[4096];  
int bytesRead;  
while ((bytesRead = is.read(buffer)) != -1) {  
os.write(buffer, 0, bytesRead);  
}  
is.close();  
os.close();  
  
Log.e(TAG, "start to load file: " + mCascadeFile.getAbsolutePath());  
classifier = new CascadeClassifier(mCascadeFile.getAbsolutePath());  
  
if (classifier.empty()) {  
Log.e(TAG, "Failed to load cascade classifier");  
classifier = null;  
} else  
Log.i(TAG, "Loaded cascade classifier from " + mCascadeFile.getAbsolutePath());  
  
cascadeDir.delete();  
  
  
} catch (IOException e) {  
e.printStackTrace();  
Log.e(TAG, "Failed to load cascade. Exception thrown: " + e);  
}  
return classifier;  
}  
  
}
```
## 效果：
1. 检测效果不是很好，识别到的东西不太行。
2. opencv级联器好像也没有对应的识别嘴巴的。
3. 识别嘴巴和识别嘴唇有时候会混
# REF:
1. [使用OpenCV实现Android人脸检测APP - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/441438069)