---
{"tags":["Project/省中医APP开发/拍照界面"],"dg-publish":true,"permalink":"/Project/省中医APP开发/APP的舌诊报告界面/","dgPassFrontmatter":true}
---

# 报告界面：
应该还是用组合函数+ [[Project/AndroidStudio/ViewModel\|ViewModel]] +[[Project/AndroidStudio/Navigation\|Navigation]]来做吧...，

## ViewModel基本设置
```kotlin
//1. 先设置报告的基本数据信息：患者姓名、舌象分析、舌象报告、患者图片
@Parcelize  
data class ReportData(  
	val title: String,  
	val content: String,  
	val image: Bitmap, // 使用 URI 字符串来表示图片  
) : Parcelable
//2. 设置ViewModel
class ReportViewModel : ViewModel() {  
  
	private val _reportData: MutableState<ReportData?> = mutableStateOf(null)  
		val reportData: State<ReportData?> = _reportData  
	  
	fun loadReportData(reportData: ReportData) {  
		_reportData.value = reportData  
	}  
}
```
## Navigation设置
```kotlin
val reportViewModel: ReportViewModel = viewModel() //在顶层函数设置，然后依次传递下去

@coil.annotation.ExperimentalCoilApi  
@Composable  
fun NavigationHost(navController: NavHostController,reportViewModel: ReportViewModel){  
	NavHost(navController=navController, startDestination = NavigationItem.Home.route) {  
	//主页的三个屏幕  
		composable(NavigationItem.Home.route) {  
			HomeScreen(navController)  
		}  
		composable(NavigationItem.Favorites.route,) {  
			FavoritesScreen(navController,reportViewModel)  
		}  
		composable(NavigationItem.Profile.route) {  
			ProfileScreen()  
		}  
		//看舌页面的两个按钮导航  
		composable(NavigationItem.TakePhoto.route){  
			TakePhoto(navController,reportViewModel)  
		}  
		composable(NavigationItem.Gallery.route){  
			Gallery(navController,reportViewModel)  
		}  
		//传递Report的ViewModel数据  
		composable(NavigationItem.Report.route){  
			ReportScreen(navController, reportViewModel)  
		}  
	}  
}
```
## 界面
![6b6971b5b94220e041e435ed3520c3b.jpg|200](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/6b6971b5b94220e041e435ed3520c3b.jpg)

界面编写与其他的类似，就是多了一个步骤——依次把ViewModel中的数据读取
```kotlin
@Composable  
fun ReportScreenSection(reportViewModel: ReportViewModel){  
	val reportData by reportViewModel.reportData  
	val scrollState = rememberScrollState()  
	//重点为这句，读取数据
	reportData?.let { data ->  
	Column(  
		modifier = Modifier  
		.fillMaxSize()  
		.verticalScroll(scrollState)  
		.padding(16.dp)  
	) {  
			UserInfoSection(username = "xxx患者")  
			Spacer(modifier = Modifier.height(16.dp))  
			InitText(text = "舌象分析:", fontSize = 16.0f)  
			  
			Image(  
				bitmap = data.image.asImageBitmap(),  
				contentDescription = "Cropped Image",  
				modifier = Modifier.fillMaxSize(),  
				contentScale = ContentScale.Crop  
			)  
			InitText(text = data.title)  
			Spacer(modifier = Modifier.height(16.dp))  
			  
			// 根据 reportData 生成报告内容  
			InitText(text = data.content)  
			Spacer(modifier = Modifier.height(8.dp))  
			  
			// 显示图片  
			InitText(text = "舌象数据：", fontSize = 16.0f)  
			Spacer(modifier = Modifier.height(8.dp))  
			InitText(text = "这里出现一些具体的舌象数据")  
		}  
	} ?: run {  
	InitText(text = "No report data available")  
	}  
}
```

# 状态界面
大致效果图：
![Pasted image 20240613201012.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240613201012.png)
## 功能
1. 滑动切换页面
	1. 通过TabRow+HorizontalPager实现
2. 通过点击编辑按钮可对报告进行处理
3. 通过输入搜索框，可对报告内容进行筛选
### 舌象报告界面
1. 简略的报告界面
	1. 通过ViewModel+LazyColumn实现
	2. 实现滑动删除的功能
2. 通过点击舌象报告的这些简略报告，可以进入到报告界面
	1. 通过ViewModel+Navigate实现，但是目前的ViewModel还没有去把图片给写进去
3. 点击时间可将报告按照正序或者反序进行排列，通过ViewModel对表进行排列
```kotlin
//ViewModel的相关代码
@Parcelize  
data class ReportData(  
val patient: String = "郑栩轩",  
val analysis: String = "这是舌象分析",  
val report:String = "这是舌象报告",  
val time: String  
) : Parcelable  
  
  
class ReportViewModel : ViewModel() {  
  
//图像资源  
val bitmap = MutableLiveData<Bitmap?>()  
//进入报告时所需数据  
private val _reportData: MutableState<ReportData?> = mutableStateOf(null)  
val reportData: State<ReportData?> = _reportData  
fun loadReportData(reportData: ReportData) {  
_reportData.value = reportData  
}  
  
private val dateFormat = SimpleDateFormat("yyyy-MM-dd HH:mm:ss", Locale.getDefault())  
private val defaultDate = Date(Long.MAX_VALUE) // 默认日期值  
  
//报告列表  
private val _reportList = MutableLiveData<List<ReportData>>()  
val reportList: LiveData<List<ReportData>> = _reportList  
// 排序顺序标志  
private val _isAscendingOrder = MutableLiveData(true)  
val isAscendingOrder: LiveData<Boolean> = _isAscendingOrder  
  
/*  
报告编辑的相关方法  
*/  
init {  
// 初始化报告列表并按时间正序排序  
_reportList.value = loadReportsFromBackend().sortedBy { reportData ->  
try {  
dateFormat.parse(reportData.time) ?: defaultDate  
} catch (e: Exception) {  
e.printStackTrace()  
defaultDate  
}  
}  
}  
  
fun loadReportList(reports: List<ReportData>) {  
_reportList.value = reports  
}  
  
fun addReport(report: ReportData) {  
val currentList = _reportList.value ?: emptyList()  
_reportList.value = currentList + report  
}  
  
fun updateReport(index: Int, report: ReportData) {  
val currentList = _reportList.value?.toMutableList() ?: mutableListOf()  
if (index in currentList.indices) {  
currentList[index] = report  
_reportList.value = currentList  
}  
}  
  
fun deleteReport(report: ReportData) {  
_reportList.value = _reportList.value?.filter { it != report }  
}  
  
// 按时间排序  
fun sortReportsByTime() {  
val currentList = _reportList.value  
if (currentList != null) {  
val sortedList = currentList.sortedBy { reportData ->  
try {  
dateFormat.parse(reportData.time) ?: defaultDate  
} catch (e: Exception) {  
e.printStackTrace()  
defaultDate  
}  
}  
_reportList.value = if (_isAscendingOrder.value == true) sortedList else sortedList.reversed()  
_isAscendingOrder.value = _isAscendingOrder.value != true  
}  
}  
// 模拟从后端获取数据的函数  
private fun loadReportsFromBackend(): List<ReportData> {  
return listOf(  
ReportData(patient = "郑栩轩", analysis = "淡白舌", report = "应多吃清谈食品", time = "2024-06-11 07:31:39"),  
ReportData(patient = "钟培铭", analysis = "淡白舌", report = "应多吃清谈食品", time = "2024-06-13 07:31:39"),  
ReportData(patient = "钟培铭", analysis = "淡白舌", report = "应多吃清谈食品", time = "2024-06-07 07:31:39"),  
// 添加更多数据  
)  
}  
  
}
```

# 不足：
通过这个界面，对于ViewModel的变量以及remember这些发现还是不清楚，应当多补一补
# REF:
1. https://juejin.cn/post/7325259560523677747 Box侧滑删除功能



