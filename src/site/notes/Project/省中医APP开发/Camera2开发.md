---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/省中医APP开发/Camera2开发/","dgPassFrontmatter":true}
---

# Camera整体框架
三个进程
1. app进程
2. camera server进程
3. [[hal\|hal]]进程 (provide进程)
进程之间的通信是通过binder实现的，app和camera server的通信是通过AIDL，camera server和hal进程通过HIDL（HAL interface definition language指定HAL和用户之间接口的一种接口描述语言）
Android框架分级大体上是类似的，有APP->Framework->Hal层，camera server负责app和framework层的通信，Hal进程负责framework和Hal层之间的通信。
**Android Camera框架**
![Pasted image 20240402204112.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240402204112.png)

# Camera2和Camera1区别
1. 在开启相机之前检查相机信息
2. 在不开启预览的情况下拍照
3. 一次拍摄多张不用格式和尺寸的图片
4. 控制曝光时间
5. 连拍
6. 灵活的3A控制 3A (AF、AE、AWB)

# Camera2 API涉及类
![Pasted image 20240402205100.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240402205100.png)
1. **Pipeline**
	1. Camera2的API模型被设计成为一个Pipeline（管道），它按顺序处理每一帧的请求并返回请求结果给客户端。
	2. 拍照过程：
		1. 创建一个用于从Pipeline获取图片的CaptureRequest
		2. 修改CaptureRequest的配置
		3. 创建surface用于接收图片数据，并将他们添加到CaptureRequest
		4. 发送配置好的CaptureRequest到Pipeline等待它返回拍照结果
2. Supported Hardware Level
	1. 不同设备对Camera支持的不同等级
		1. LEGACY：只支持Camrea1的功能
		2. LIMITED：支持部分Camera2的功能
		3. FULL：支持所有Camera2的高级功能
		4. LEVEL_3：新增更多Camera2的高级特性
3. **Capture**：相机的所有操作和参数配置最终都是为了图像捕获，在Camera2中所有的相机操作和参数设置都被抽象为Capture
	1. 执行方式：
		1. 单次模式：拍一张照片，多个一次性模式的Capture会进行队列按顺序执行
		2. 多次模式（Burst）：与单次模式的最大区别是连续多次Capture器件不允许插入其他任何Capture操作
		3. 重复模式（Repeating）：不断重复执行指定的Capture操作，当有其他模式的Capture会暂停该模式，等到其他模式执行完毕后又会自动恢复为重复模式。显示预览画面就是不断Capture获取每一帧画面。
	2. 注意事项
		1. 无论Capture以何种模式被提交，他们都是按顺序串行执行的，没有并行的情况
		2. 重复模型比较特别，因为它会保留CaptureRequest对象用于不断重复Capture操作，这就导致它的参数和其他模式不一样
		3. 如果某一次的Capture没有配置预览的Surface，会导致本次Capture不会将画面输出到预览的Surface上
4. CameraManager：负责查询和建立相机连接的系统服务
	1. 将相机信息封装到CameraCharacteristics中
	2. 根据指定的相机ID连接相机设备
	3. 提供将闪光灯设置成手电筒模式的快捷方式。
5. CameraCharacteristics：一个只读的相机信息提供者，带有大量的相机信息
	1. 获取所有可用AE模式
	2. COnTROL_AE_AVAILABLE_MODES
6. CameraDevice：代表当前连接的相机设备
	1. 根据指定的参数创建 CameraCaptureSession。
	2. 根据指定的模板创建 CaptureRequest。
	3. 关闭相机设备。
	4. 监听相机设备的状态，例如断开连接、开启成功和开启失败等。
7. Surface：一块用于填充图像数据的内存控件，可以用SurfaceView的Surface接收每一帧预览数据用于显示预览画面，也可以用ImageReader的Surface接收JPEG或YUV数据
8. CameraCaptureSession: 配置了目标Surface的Pipeline实例，在使用相机功能之前必须先创建CameraCaptureSession实例。绝大多数相机操作都是通过CameraCaptureSession提交一个Capture实现的
9. CaptureRequest：是向CameraCaptureSession提交Capture请求时的信息载体，CaptureRequest 可以配置的信息非常多，包括图像格式、图像分辨率、传感器控制、闪光灯控制、3A 控制等等，可以说绝大部分的相机参数都是通过 CaptureRequest 配置的。每一个 CaptureRequest 表示一帧画面的操作，这意味着你可以精确控制每一帧的 Capture 操作。
10. CaptureResult：是每一次Capture操作的结果，里面包括了很多状态信息，里面包括了很多状态信息，包括闪光灯状态、对焦状态、时间戳等等。

<style> .container {font-family: sans-serif; text-align: center;} .button-wrapper button {z-index: 1;height: 40px; width: 100px; margin: 10px;padding: 5px;} .excalidraw .App-menu_top .buttonList { display: flex;} .excalidraw-wrapper { height: 800px; margin: 50px; position: relative;} :root[dir="ltr"] .excalidraw .layer-ui__wrapper .zen-mode-transition.App-menu_bottom--transition-left {transform: none;} </style><script src="https://cdn.jsdelivr.net/npm/react@17/umd/react.production.min.js"></script><script src="https://cdn.jsdelivr.net/npm/react-dom@17/umd/react-dom.production.min.js"></script><script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@excalidraw/excalidraw@0/dist/excalidraw.production.min.js"></script><div id="Drawing_2024-04-02_2118.00.excalidraw.md1"></div><script>(function(){const InitialData={"type":"excalidraw","version":2,"source":"https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.25","elements":[{"id":"KDZwOptp","type":"embeddable","x":-699.4609852404695,"y":-499.7838991210988,"width":949.4726895015257,"height":1619.8049743461615,"angle":0,"strokeColor":"transparent","backgroundColor":"transparent","fillStyle":"hachure","strokeWidth":1,"strokeStyle":"solid","roughness":1,"opacity":100,"roundness":null,"seed":4666,"version":1064,"versionNonce":860465338,"updated":1712065197757,"isDeleted":false,"groupIds":[],"boundElements":[],"link":"[[原生Camera开发#Camera2 API涉及类\|原生Camera开发#Camera2 API涉及类]]","locked":false,"scale":[1,1],"customData":{"mdProps":{"useObsidianDefaults":false,"backgroundMatchCanvas":false,"backgroundMatchElement":true,"backgroundColor":"#fff","backgroundOpacity":60,"borderMatchElement":true,"borderColor":"#fff","borderOpacity":0,"filenameVisible":false}}},{"id":"bcNxpZ7p","type":"text","x":757.1108395444539,"y":199.66346804464968,"width":258.36309814453125,"height":82.5196071585645,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":null,"seed":258294566,"version":387,"versionNonce":1881991142,"isDeleted":false,"boundElements":[{"id":"L0F0GYSOmjzvNzF2plq1y","type":"arrow"},{"id":"L4cj6SUOkUzOSDmjGPMg3","type":"arrow"}],"updated":1712064939832,"link":null,"locked":false,"text":"Capture","rawText":"Capture","fontSize":66.0156857268516,"fontFamily":1,"textAlign":"left","verticalAlign":"top","baseline":57,"containerId":null,"originalText":"Capture","lineHeight":1.25},{"id":"sWQ6jCxB","type":"text","x":767.2613584547894,"y":-314.6294900789869,"width":194.66490173339844,"height":63.91032248961708,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":null,"seed":1039303482,"version":207,"versionNonce":860341734,"isDeleted":false,"boundElements":[{"id":"CxXj1XcIXcthTnIBgWH5n","type":"arrow"}],"updated":1712064514501,"link":null,"locked":false,"text":"Surface","rawText":"Surface","fontSize":51.128257991693665,"fontFamily":1,"textAlign":"left","verticalAlign":"top","baseline":44,"containerId":null,"originalText":"Surface","lineHeight":1.25},{"id":"stpdPLuI","type":"text","x":797.7129151857941,"y":377.2975489755107,"width":328.7076721191406,"height":166.7080818946455,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":null,"seed":2109453670,"version":292,"versionNonce":794230394,"isDeleted":false,"boundElements":null,"updated":1712065167994,"link":null,"locked":false,"text":"CaptureSession\n相机的主要操作\n","rawText":"CaptureSession\n相机的主要操作\n","fontSize":44.4554885052388,"fontFamily":1,"textAlign":"left","verticalAlign":"top","baseline":149,"containerId":null,"originalText":"CaptureSession\n相机的主要操作\n","lineHeight":1.25},{"id":"rBYAKRt6","type":"text","x":381.5416398620621,"y":-136.99540914812565,"width":156.14527893066406,"height":57.143309882727195,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":null,"seed":810795238,"version":138,"versionNonce":1223499130,"isDeleted":false,"boundElements":[{"id":"3CpTZiF343YG4SILCSHeP","type":"arrow"},{"id":"dV3MiEvGpogR-OdXiVB_9","type":"arrow"}],"updated":1712064498173,"link":null,"locked":false,"text":"Pipeline","rawText":"Pipeline","fontSize":45.71464790618175,"fontFamily":1,"textAlign":"left","verticalAlign":"top","baseline":40,"containerId":null,"originalText":"Pipeline","lineHeight":1.25},{"id":"kZ4dEiUE","type":"text","x":259.73541293804305,"y":94.77477263785522,"width":457.79400634765625,"height":72.3690882482297,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":null,"seed":26369830,"version":96,"versionNonce":1590994534,"isDeleted":false,"boundElements":[{"id":"3CpTZiF343YG4SILCSHeP","type":"arrow"},{"id":"L4cj6SUOkUzOSDmjGPMg3","type":"arrow"}],"updated":1712064939832,"link":null,"locked":false,"text":"CaptureRequest","rawText":"CaptureRequest","fontSize":57.89527059858376,"fontFamily":1,"textAlign":"left","verticalAlign":"top","baseline":50,"containerId":null,"originalText":"CaptureRequest","lineHeight":1.25},{"type":"text","version":179,"versionNonce":1356491174,"isDeleted":false,"id":"c3NGofh6","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"angle":0,"x":741.3540522671477,"y":-142.72839654123572,"strokeColor":"#1e1e1e","backgroundColor":"transparent","width":409.745361328125,"height":144.7381764964594,"seed":1826540218,"groupIds":[],"frameId":null,"roundness":null,"boundElements":[],"updated":1712065187113,"link":null,"locked":false,"fontSize":57.89527059858376,"fontFamily":1,"text":"CaptureResult\n状态信息","rawText":"CaptureResult\n状态信息","textAlign":"left","verticalAlign":"top","containerId":null,"originalText":"CaptureResult\n状态信息","lineHeight":1.25,"baseline":122},{"id":"3CpTZiF343YG4SILCSHeP","type":"arrow","x":436.9984857817026,"y":86.31600687924288,"width":3.6796734071997435,"height":160.71654941363636,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":{"type":2},"seed":185457702,"version":66,"versionNonce":203468922,"isDeleted":false,"boundElements":null,"updated":1712064529381,"link":null,"locked":false,"points":[[0,0],[3.6796734071997435,-160.71654941363636]],"lastCommittedPoint":null,"startBinding":{"elementId":"kZ4dEiUE","focus":-0.22730583479143127,"gap":8.45876575861233},"endBinding":{"elementId":"rBYAKRt6","focus":0.23063485882026977,"gap":5.4515567310049775},"startArrowhead":null,"endArrowhead":"arrow"},{"id":"dV3MiEvGpogR-OdXiVB_9","type":"arrow","x":547.3334487308659,"y":-109.92735872056573,"width":170.86706832397113,"height":1.6917531517224234,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":{"type":2},"seed":38584678,"version":51,"versionNonce":659670202,"isDeleted":false,"boundElements":null,"updated":1712064498173,"link":null,"locked":false,"points":[[0,0],[170.86706832397113,-1.6917531517224234]],"lastCommittedPoint":null,"startBinding":{"elementId":"rBYAKRt6","focus":-0.021642713624424385,"gap":9.646529938139793},"endBinding":null,"startArrowhead":null,"endArrowhead":"arrow"},{"id":"CxXj1XcIXcthTnIBgWH5n","type":"arrow","x":447.5200127792391,"y":-158.98820012051783,"width":311.2825799169377,"height":130.2649926826316,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":{"type":2},"seed":1823858918,"version":144,"versionNonce":1603537574,"isDeleted":false,"boundElements":null,"updated":1712064514501,"link":null,"locked":false,"points":[[0,0],[16.917531517224916,-128.57323953090918],[311.2825799169377,-130.2649926826316]],"lastCommittedPoint":null,"startBinding":null,"endBinding":{"elementId":"sWQ6jCxB","focus":0.22103522828142114,"gap":8.458765758612572},"startArrowhead":null,"endArrowhead":"arrow"},{"id":"RHwZx75k","type":"text","x":313.8715137931629,"y":937.2678421956541,"width":524.5396728515625,"height":171.8062269240191,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":null,"seed":439343782,"version":258,"versionNonce":2081720102,"isDeleted":false,"boundElements":[{"id":"wcz_-1s8BEegpzzbTwuE5","type":"arrow"}],"updated":1712065104747,"link":null,"locked":false,"text":"CameraManager\n连接相机设备","rawText":"CameraManager\n连接相机设备","fontSize":68.72249076960763,"fontFamily":1,"textAlign":"left","verticalAlign":"top","baseline":146,"containerId":null,"originalText":"CameraManager\n连接相机设备","lineHeight":1.25},{"id":"RCAN7EKj","type":"text","x":895.834597985698,"y":764.7090207199601,"width":463.35614013671875,"height":104.1361008551196,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":null,"seed":1201972218,"version":400,"versionNonce":992280698,"isDeleted":false,"boundElements":[{"id":"2Sm7qZ28vXblbd2EogW1M","type":"arrow"}],"updated":1712065087772,"link":null,"locked":false,"text":"CameraCharacteristics\n查看硬件，相机信息","rawText":"CameraCharacteristics\n查看硬件，相机信息","fontSize":41.654440342047835,"fontFamily":1,"textAlign":"left","verticalAlign":"top","baseline":88,"containerId":null,"originalText":"CameraCharacteristics\n查看硬件，相机信息","lineHeight":1.25},{"id":"tB2Vx343","type":"text","x":442.4447533240716,"y":475.419231775415,"width":297.47808837890625,"height":55.451556731004786,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":null,"seed":1007148218,"version":151,"versionNonce":934690362,"isDeleted":false,"boundElements":[{"id":"GL8FD0N_u2hk4z6TLD_wQ","type":"arrow"},{"id":"wcz_-1s8BEegpzzbTwuE5","type":"arrow"}],"updated":1712065016705,"link":null,"locked":false,"text":"CameraDevice","rawText":"CameraDevice","fontSize":44.36124538480383,"fontFamily":1,"textAlign":"left","verticalAlign":"top","baseline":38,"containerId":null,"originalText":"CameraDevice","lineHeight":1.25},{"id":"GL8FD0N_u2hk4z6TLD_wQ","type":"arrow","x":582.860264917038,"y":470.34397232024753,"width":55.82785400684202,"height":284.21452948937804,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":{"type":2},"seed":1575453606,"version":34,"versionNonce":1392052666,"isDeleted":false,"boundElements":null,"updated":1712064869478,"link":null,"locked":false,"points":[[0,0],[-55.82785400684202,-284.21452948937804]],"lastCommittedPoint":null,"startBinding":{"elementId":"tB2Vx343","focus":-0.012196189059438643,"gap":5.075259455167497},"endBinding":null,"startArrowhead":null,"endArrowhead":"arrow"},{"id":"yBWAqkVl5y6VEEE7kMGKL","type":"arrow","x":623.4623405583777,"y":441.5841687409652,"width":153.94953680674644,"height":23.684544124114836,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":{"type":2},"seed":2071552826,"version":115,"versionNonce":1995180602,"isDeleted":false,"boundElements":null,"updated":1712064928605,"link":null,"locked":false,"points":[[0,0],[153.94953680674644,-23.684544124114836]],"lastCommittedPoint":null,"startBinding":null,"endBinding":null,"startArrowhead":null,"endArrowhead":"arrow"},{"id":"L0F0GYSOmjzvNzF2plq1y","type":"arrow","x":887.3758322270854,"y":355.30475800311865,"width":3.3835063034448467,"height":60.903113462009514,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":{"type":2},"seed":706294182,"version":25,"versionNonce":482657530,"isDeleted":false,"boundElements":null,"updated":1712064936444,"link":null,"locked":false,"points":[[0,0],[-3.3835063034448467,-60.903113462009514]],"lastCommittedPoint":null,"startBinding":null,"endBinding":{"elementId":"bcNxpZ7p","focus":0.04009228453932961,"gap":12.218569337894962},"startArrowhead":null,"endArrowhead":"arrow"},{"id":"L4cj6SUOkUzOSDmjGPMg3","type":"arrow","x":892.4510916822529,"y":194.5882085894823,"width":167.48356202052628,"height":72.74538552406693,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":{"type":2},"seed":1199525498,"version":96,"versionNonce":1116419898,"isDeleted":false,"boundElements":null,"updated":1712064941736,"link":null,"locked":false,"points":[[0,0],[-3.383506303445074,-69.36187922062186],[-167.48356202052628,-72.74538552406693]],"lastCommittedPoint":null,"startBinding":{"elementId":"bcNxpZ7p","focus":0.06417165243775595,"gap":5.075259455167384},"endBinding":{"elementId":"kZ4dEiUE","focus":-0.34200466233474036,"gap":7.438110376027339},"startArrowhead":null,"endArrowhead":"arrow"},{"id":"2Sm7qZ28vXblbd2EogW1M","type":"arrow","x":745.268567482398,"y":947.1832238815598,"width":142.81352122100031,"height":138.15869741072277,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":{"type":2},"seed":1278329402,"version":577,"versionNonce":1046667578,"isDeleted":false,"boundElements":null,"updated":1712065087773,"link":null,"locked":false,"points":[[0,0],[142.81352122100031,-138.15869741072277]],"lastCommittedPoint":null,"startBinding":null,"endBinding":{"elementId":"RCAN7EKj","focus":0.8667039706120987,"gap":7.752509282299741},"startArrowhead":null,"endArrowhead":"arrow"},{"id":"wcz_-1s8BEegpzzbTwuE5","type":"arrow","x":635.7641750646279,"y":930.5008295887653,"width":21.628319198836266,"height":398.63004108234554,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":{"type":2},"seed":1981881574,"version":311,"versionNonce":2051896742,"isDeleted":false,"boundElements":null,"updated":1712065104749,"link":null,"locked":false,"points":[[0,0],[-21.628319198836266,-398.63004108234554]],"lastCommittedPoint":null,"startBinding":{"elementId":"RHwZx75k","focus":0.2422715190790282,"gap":6.76701260688867},"endBinding":{"elementId":"tB2Vx343","focus":-0.1423922888748802,"gap":1},"startArrowhead":null,"endArrowhead":"arrow"},{"id":"0tjO0Smc","type":"text","x":770.6448647582343,"y":-373.84085038927276,"width":166.5999755859375,"height":52.068050427559776,"angle":0,"strokeColor":"#1e1e1e","backgroundColor":"transparent","fillStyle":"solid","strokeWidth":2,"strokeStyle":"solid","roughness":1,"opacity":100,"groupIds":[],"frameId":null,"roundness":null,"seed":1974672806,"version":68,"versionNonce":1231962682,"isDeleted":false,"boundElements":null,"updated":1712065233743,"link":null,"locked":false,"text":"图像数据","rawText":"图像数据","fontSize":41.65444034204782,"fontFamily":1,"textAlign":"left","verticalAlign":"top","baseline":36,"containerId":null,"originalText":"图像数据","lineHeight":1.25}],"appState":{"theme":"light","viewBackgroundColor":"#ffffff","currentItemStrokeColor":"#1e1e1e","currentItemBackgroundColor":"transparent","currentItemFillStyle":"solid","currentItemStrokeWidth":2,"currentItemStrokeStyle":"solid","currentItemRoughness":1,"currentItemOpacity":100,"currentItemFontFamily":1,"currentItemFontSize":20,"currentItemTextAlign":"left","currentItemStartArrowhead":null,"currentItemEndArrowhead":"arrow","scrollX":623.3597322610955,"scrollY":974.0431482488165,"zoom":{"value":0.591102785286277},"currentItemRoundness":"round","gridSize":null,"gridColor":{"Bold":"#C9C9C9FF","Regular":"#EDEDEDFF"},"currentStrokeOptions":null,"previousGridSize":null,"frameRendering":{"enabled":true,"clip":true,"name":true,"outline":true}},"files":{}};InitialData.scrollToContent=true;App=()=>{const e=React.useRef(null),t=React.useRef(null),[n,i]=React.useState({width:void 0,height:void 0});return React.useEffect(()=>{i({width:t.current.getBoundingClientRect().width,height:t.current.getBoundingClientRect().height});const e=()=>{i({width:t.current.getBoundingClientRect().width,height:t.current.getBoundingClientRect().height})};return window.addEventListener("resize",e),()=>window.removeEventListener("resize",e)},[t]),React.createElement(React.Fragment,null,React.createElement("div",{className:"excalidraw-wrapper",ref:t},React.createElement(ExcalidrawLib.Excalidraw,{ref:e,width:n.width,height:n.height,initialData:InitialData,viewModeEnabled:!0,zenModeEnabled:!0,gridModeEnabled:!1})))},excalidrawWrapper=document.getElementById("Drawing_2024-04-02_2118.00.excalidraw.md1");ReactDOM.render(React.createElement(App),excalidrawWrapper);})();</script>


# 相机项目前期准备
1. 注册相关权限
```java
//注册相关权限
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
	 <!-- 打开相机权限 -->
    <uses-permission android:name="android.permission.CAMERA" />  
    <uses-feature android:name="android.hardware.camera" android:required="true" />
    <!-- 读写权限 -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
	<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />  
	<uses-permission android:name="android.permission.MANAGE_EXTERNAL_STORAGE" tools:ignore="ScopedStorage" />
</manifest>
```
2. [[Project/AndroidStudio/动态权限申请\|动态权限申请]]
```java
public class MainActivity extends AppCompatActivity {
    private static final int REQUEST_PERMISSION_CODE = 1;
    private static final String[] REQUIRED_PERMISSIONS = {
            Manifest.permission.CAMERA,
            Manifest.permission.WRITE_EXTERNAL_STORAGE
    };

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        checkRequiredPermissions();
    }

    /*
     * 判断我们需要的权限是否被授予，只要有一个没有授权，我们都会进行权限申请操作。
     * @return true 权限都被授权
     */
    private boolean checkRequiredPermissions() {
        List<String> deniedPermissions = new ArrayList<>();
        for (String permission : REQUIRED_PERMISSIONS) {
            if (ContextCompat.checkSelfPermission(this, permission) == PackageManager.PERMISSION_DENIED) {
                deniedPermissions.add(permission);//将拒绝的权限添加到列表中
            }
        }
        if (!deniedPermissions.isEmpty()) {
            ActivityCompat.requestPermissions(this, deniedPermissions.toArray(new String[0]), REQUEST_PERMISSION_CODE);
        }
        return deniedPermissions.isEmpty();
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        boolean permissionDenied = false;  
		for (int result : grantResults) {  
			if (result == PackageManager.PERMISSION_DENIED) {  
				permissionDenied = true;  
				break;  
			}  
		}  
		super.onRequestPermissionsResult(requestCode, permissions, grantResults);  
		if (requestCode == REQUEST_CAMERA_PERMISSION) {  
			// 权限被拒绝  
			if (permissionDenied &&!ActivityCompat.shouldShowRequestPermissionRationale(this, Manifest.permission.CAMERA)) {  
			// 用户勾选 "不再询问" 选项，向用户解释权限的重要性并再次请求权限  
			showPermissionSettingsDialog();  
			}  
		}  
	}
	private void showPermissionSettingsDialog() {  
		// 显示一个对话框，引导用户前往应用设置页面手动开启权限  
		AlertDialog.Builder builder = new AlertDialog.Builder(this);  
		builder.setMessage("您已经拒绝了相机权限，您可以在应用设置中手动开启权限。");  
		builder.setPositiveButton("去设置", (dialog,which)-> {  
			Intent intent = new Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS);  
			Uri uri = Uri.fromParts("package", getPackageName(), null);  
			intent.setData(uri);  
			startActivity(intent);  
		});  
		builder.setNegativeButton("取消", null);  
		builder.show();  
	}
```
3. 开启相机前准备
```java
private String frontCameraId; 
private CameraCharacteristics frontCameraCharacteristics;
private String backCameraId; 
private CameraCharacteristics backCameraCharacteristics;

//获取CameraManager实例
private CameraManager cameraManager = (CameraManager) getSystemService(Context.CAMERA_SERVICE);
// 获取相机ID列表
String[] cameraIdList = new String[0];
try {
    cameraIdList = cameraManager.getCameraIdList();
} catch (CameraAccessException e) {
    e.printStackTrace();
}
private static final int REQUIRED_SUPPORTED_HARDWARE_LEVEL = CameraCharacteristics.INFO_SUPPORTED_HARDWARE_LEVEL_FULL;
// 判断相机的 Hardware Level 是否大于等于指定的 Level。
public boolean isHardwareLevelSupported(CameraCharacteristics characteristics, int requiredLevel) {
    int[] sortedLevels = {
            CameraCharacteristics.INFO_SUPPORTED_HARDWARE_LEVEL_LEGACY,
            CameraCharacteristics.INFO_SUPPORTED_HARDWARE_LEVEL_LIMITED,
            CameraCharacteristics.INFO_SUPPORTED_HARDWARE_LEVEL_FULL,
            CameraCharacteristics.INFO_SUPPORTED_HARDWARE_LEVEL_3
    };
    Integer deviceLevel = characteristics.get(CameraCharacteristics.INFO_SUPPORTED_HARDWARE_LEVEL);
    if (requiredLevel == deviceLevel) {
        return true;//当所需级别和设备级别相等，直接返回true
    }
    //如果所需级别和设备级别不相等，遍历以排序的级别数据
    //好帅的排序，差点看懂
    for (int sortedLevel : sortedLevels) {
        if (requiredLevel == sortedLevel) {
            return true;
        } else if (deviceLevel == sortedLevel) {
            return false;
        }
    }
    return false;
}
/*CameraCharacteristics 是相机信息的提供者，通过它我们可以获取所有相机信息，
这里我们需要根据摄像头的方向筛选出前置和后置摄像头，
并且要求相机的 Hardware Level 必须是 FULL 及以上，
所以首先我们要获取所有相机的 CameraCharacteristics 实例，
涉及的 API 是 CameraManager.getCameraCharacteristics()，
它会根据你指定的相机 ID 返回对应的相机信息：*/
// 遍历所有可用的摄像头 ID，只取出其中的前置和后置摄像头信息。
try {
    cameraIdList = cameraManager.getCameraIdList();//获取所有相机的id
    for (String cameraId : cameraIdList) {
        CameraCharacteristics cameraCharacteristics = cameraManager.getCameraCharacteristics(cameraId);//获取相机
        if (isHardwareLevelSupported(cameraCharacteristics, REQUIRED_SUPPORTED_HARDWARE_LEVEL)) {
            Integer facing = cameraCharacteristics.get(CameraCharacteristics.LENS_FACING);
            if (facing != null && facing == CameraCharacteristics.LENS_FACING_FRONT) {
                frontCameraId = cameraId;
                frontCameraCharacteristics = cameraCharacteristics;
            } else if (facing != null && facing == CameraCharacteristics.LENS_FACING_BACK) {
                backCameraId = cameraId;
                backCameraCharacteristics = cameraCharacteristics;
            }
        }
    }
} catch (CameraAccessException e) {
    e.printStackTrace();
}


```
4. 开启相机
	1. 用 `CameraManager.openCamera () ` 方法开启相机，该方法要求我们传递两个参数，一个是相机 ID，一个是监听相机状态的 `CameraStateCallback`。当相机被成功开启的时候会通过 `CameraStateCallback.onOpened ()` 方法回调一个 `CameraDevice` 实例给你，否则的话会通过 `CameraStateCallback.onError ()` 方法回调一个 `CameraDevice` 实例和一个错误码给你。`onOpened ()` 和 `onError () ` 其实都意味着相机已经被开启了，唯一的区别是 `onError () ` 表示开启过程中出了问题，你必须把传递给你的 ` CameraDevice` 关闭，而不是继续使用它。
	2. 在Android开发中，相机操作通常涉及到一些耗时的操作，例如打开相机、设置相机参数、拍摄照片等。如果在主线程（UI线程）中执行这些操作，可能会导致界面卡顿、响应性能下降甚至应用无响应。因此，建议将这些相机操作放在单独的线程中执行，以避免阻塞主线程。
	3. 下面是主要代码片段：
```java
private void openCamera() {
    // 首选后置摄像头，如果没有则使用前置摄像头。
    String cameraId = backCameraId != null ? backCameraId : frontCameraId;
    if (cameraId != null) {
        OpenCameraMessage openCameraMessage = new OpenCameraMessage(cameraId, new CameraStateCallback());
        Message message = cameraHandler.obtainMessage(MSG_OPEN_CAMERA, openCameraMessage);
        message.sendToTarget();
    } else {
        throw new RuntimeException("Camera id must not be null.");
    }
}

private static class OpenCameraMessage {
    private String cameraId;
    private CameraStateCallback cameraStateCallback;

    public OpenCameraMessage(String cameraId, CameraStateCallback cameraStateCallback) {
        this.cameraId = cameraId;
        this.cameraStateCallback = cameraStateCallback;
    }

    public String getCameraId() {
        return cameraId;
    }
	//
    public CameraStateCallback getCameraStateCallback() {
        return cameraStateCallback;
    }
}

@Override
public boolean handleMessage(Message msg) {
    switch (msg.what) {
        case MSG_OPEN_CAMERA:
            OpenCameraMessage openCameraMessage = (OpenCameraMessage) msg.obj;
            String cameraId = openCameraMessage.getCameraId();
            CameraStateCallback cameraStateCallback = openCameraMessage.getCameraStateCallback();
            try {
                cameraManager.openCamera(cameraId, cameraStateCallback, cameraHandler);
                Log.d(TAG, "Handle message: MSG_OPEN_CAMERA");
            } catch (CameraAccessException e) {
                e.printStackTrace();
            }
            break;
    }
    return false;
}


private class CameraStateCallback extends CameraDevice.StateCallback {
    @Override
    public void onOpened(@NonNull CameraDevice camera) {
        cameraDevice = camera;
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(MainActivity.this, "相机已开启", Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    public void onError(@NonNull CameraDevice camera, int error) {
        camera.close();
        cameraDevice = null;
    }
}

```
5. 关闭相机
```java
@SuppressLint("MissingPermission")
@Override
public boolean handleMessage(Message msg) {
    switch (msg.what) {
        case MSG_CLOSE_CAMERA:
            if (cameraDevice != null) {
                cameraDevice.close();
                Log.d(TAG, "Handle message: MSG_CLOSE_CAMERA");
            }
            break;
    }
    return false;
}

@Override
protected void onPause() {
    super.onPause();
    closeCamera();
}

private void closeCamera() {
    if (cameraHandler != null) {
        cameraHandler.sendEmptyMessage(MSG_CLOSE_CAMERA);
    }
}

private class CameraStateCallback extends CameraDevice.StateCallback {
    @Override
    public void onClosed(@NonNull CameraDevice camera) {
        cameraDevice = null;
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(MainActivity.this, "相机已关闭", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```
6. 获取预览尺寸
	1.  `CameraCharacteristics`是一个只读的相机信息提供者，其内部携带大量的相机信息，包括代表相机朝向的 LENS_FACING；判断闪光灯是否可用的 FLASH_INFO_AVAILABLE；获取所有可用 AE 模式的 CONTROL_AE_AVAILABLE_MODES 等等。通过 `CameraCharacteristics.get()` 方法获取相机信息，该方法要求你传递一个 Key 以确定你要获取哪方面的相机信息
	2. 预览尺寸列表并不是直接从 `CameraCharacteristics` 获取的，而是先通过 `SCALER_STREAM_CONFIGURATION_MAP` 获取 `StreamConfigurationMap` 对象，然后通过 `StreamConfigurationMap.getOutputSizes ()` 方法获取尺寸列表，该方法会要求你传递一个 `Class` 类型，然后根据这个类型返回对应的尺寸列表，如果给定的类型不支持，则返回 null，你可以通过 `StreamConfigurationMap.isOutputSupportedFor ()` 方法判断某一个类型是否被支持，常见的类型有：
		1. ImageReader：常用来拍照或接收 YUV 数据。
		2. MediaRecorder：常用来录制视频。
		3. MediaCodec：常用来录制视频。
		4. SurfaceHolder：常用来显示预览画面。
		5. SurfaceTexture：常用来显示预览画面。
```java
// 获取摄像头信息
CameraCharacteristics cameraCharacteristics = null;
try {
    cameraCharacteristics = cameraManager.getCameraCharacteristics(cameraId);
    Integer lensFacing = cameraCharacteristics.get(CameraCharacteristics.LENS_FACING);
    if (lensFacing != null) {
        switch (lensFacing) {
            case CameraCharacteristics.LENS_FACING_FRONT:
                // 前置摄像头 
                break;
            case CameraCharacteristics.LENS_FACING_BACK:
                // 后置摄像头 
                break;
            case CameraCharacteristics.LENS_FACING_EXTERNAL:
                // 外置摄像头 
                break;
        }
    }
} catch (CameraAccessException e) {
    e.printStackTrace();
}

// 通过 CameraCharacteristic 获取设备支持的所有尺寸
if (cameraCharacteristics != null) {
    StreamConfigurationMap streamConfigurationMap = cameraCharacteristics.get(CameraCharacteristics.SCALER_STREAM_CONFIGURATION_MAP);
    if (streamConfigurationMap != null) {
        Size[] supportedSizes = streamConfigurationMap.getOutputSizes(SurfaceTexture.class);
    }
}
//过滤尺寸
@WorkerThread
private Size getOptimalSize(CameraCharacteristics cameraCharacteristics, Class<?> clazz, int maxWidth, int maxHeight) {
    float aspectRatio = (float) maxWidth / maxHeight;
    StreamConfigurationMap streamConfigurationMap = cameraCharacteristics.get(CameraCharacteristics.SCALER_STREAM_CONFIGURATION_MAP);
    if (streamConfigurationMap != null) {
        Size[] supportedSizes = streamConfigurationMap.getOutputSizes(clazz);
        if (supportedSizes != null) {
            for (Size size : supportedSizes) {
                if (Math.abs(size.getWidth() / (float) size.getHeight() - aspectRatio) < ASPECT_RATIO_TOLERANCE &&
                        size.getHeight() <= maxHeight && size.getWidth() <= maxWidth) {
                    return size;
                }
            }
        }
    }
    return null;
}
```
7. 配置预览尺寸
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
 
    <TextureView
        android:id="@+id/camera_preview"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
 
</androidx.constraintlayout.widget.ConstraintLayout>
```
8. 获取`TextureView`对象，并且注册`TextureView.SurfaceTextureListener` 用于监听 `SurfaceTexture` 的状态
```java
private class PreviewSurfaceTextureListener implements TextureView.SurfaceTextureListener {
    @MainThread
    @Override
    public void onSurfaceTextureSizeChanged(SurfaceTexture surfaceTexture, int width, int height) {}

    @MainThread
    @Override
    public void onSurfaceTextureUpdated(SurfaceTexture surfaceTexture) {}

    @MainThread
    @Override
    public boolean onSurfaceTextureDestroyed(SurfaceTexture surfaceTexture) {
        return false;
    }

    @MainThread
    @Override
    public void onSurfaceTextureAvailable(SurfaceTexture surfaceTexture, int width, int height) {
        previewSurfaceTexture = surfaceTexture;
    }
}

PreviewSurfaceTextureListener listener = new PreviewSurfaceTextureListener();
cameraPreview = findViewById(R.id.camera_preview);
cameraPreview.setSurfaceTextureListener(listener);
Size previewSize = getOptimalSize(cameraCharacteristics, SurfaceTexture.class, width, height);
previewSurfaceTexture.setDefaultBufferSize(previewSize.getWidth(), previewSize.getHeight());
previewSurface = new Surface(previewSurfaceTexture);
```
9. 创建`CameraCaptureSession`
	1. 使用这个 Surface 创建一个 `CameraCaptureSession `实例，涉及的方法是 `CameraDevice.createCaptureSession ()`，该方法要求你传递以下三个参数：
		1. outputs：所有用于接收图像数据的 Surface，例如本章用于接收预览画面的 Surface，后续还会有用于拍照的 Surface，这些 Surface 必须在创建 Session 之前就准备好，并且在创建 Session 的时候传递给底层用于配置 Pipeline。
		2. callback：用于监听 Session 状态的 `CameraCaptureSession`. `StateCallback` 对象，就如同开关相机一样，创建和销毁 Session 也需要我们注册一个状态监听器。
		3. handler：用于执行 `CameraCaptureSession`. `StateCallback `的 Handler 对象，可以是异步线程的` Handler`，也可以是主线程的 `Handler`，在我们的 Demo 里使用的是主线程 Handler。
```java
private class SessionStateCallback extends CameraCaptureSession.StateCallback {
    @Override
    @MainThread
    public void onConfigureFailed(CameraCaptureSession session) {
    }

    @Override
    @MainThread
    public void onConfigured(CameraCaptureSession session) {
    }

    @Override
    @MainThread
    public void onClosed(CameraCaptureSession session) {
    }
}

SessionStateCallback sessionStateCallback = new SessionStateCallback();
List<Surface> outputs = Arrays.asList(previewSurface);
try {
    cameraDevice.createCaptureSession(outputs, sessionStateCallback, mainHandler);
} catch (CameraAccessException e) {
    e.printStackTrace();
}
```
10. 创建`CaptureRequest`
	1. CaptureRequest，因为它是我们执行任何相机操作都绕不开的核心类，因为 `CaptureRequest` 是向` CameraCaptureSession` 提交 Capture 请求时的信息载体，其内部包括了本次 Capture 的参数配置和接收图像数据的 Surface。`CaptureRequest `可以配置的信息非常多，包括图像格式、图像分辨率、传感器控制、闪光灯控制、3A 控制等等，可以说绝大部分的相机参数都是通过 `CaptureRequest` 配置的。可以通过` CameraDevice.createCaptureRequest () `方法创建一个 `CaptureRequest`. Builder 对象，该方法只有一个参数 `templateType` 用于指定使用何种模板创建 `CaptureRequest.Builder `对象。因为` CaptureRequest` 可以配置的参数实在是太多了，如果每一个参数都要去配置，那真的是既复杂又费时，所以 Camera2 根据使用场景的不同，为我们事先配置好了一些常用的参数模板：
	2. TEMPLATE_PREVIEW：适用于配置预览的模板。
	3. TEMPLATE_RECORD：适用于视频录制的模板。
	4. TEMPLATE_STILL_CAPTURE：适用于拍照的模板。
	5. TEMPLATE_VIDEO_SNAPSHOT：适用于在录制视频过程中支持拍照的模板。
	6. TEMPLATE_MANUAL：适用于希望自己手动配置大部分参数的模板。
```
CaptureRequest.Builder requestBuilder = cameraDevice.createCaptureRequest(CameraDevice.TEMPLATE_PREVIEW); // 创建一个预览的 //CaptureRequest
requestBuilder.addTarget(previewSurface); // 将预览表面添加为目标
CaptureRequest request = requestBuilder.build(); // 创建一个只读的 CaptureRequest 实例
```

11. 开启和停止预览
	1. 在 `Camera2` 里，预览本质上是不断重复执行的 Capture 操作，每一次 Capture 都会把预览画面输出到对应的 Surface 上，涉及的方法是 `CameraCaptureSession.setRepeatingRequest ()`，该方法有三个参数：
	2. request：在不断重复执行 Capture 时使用的 `CaptureRequest` 对象。
	3. callback：监听每一次 Capture 状态的 `CameraCaptureSession.CaptureCallback` 对象，例如 `onCaptureStarted ()` 意味着一次 Capture 的开始，而 `onCaptureCompleted ()` 意味着一次 Capture 的结束。
	4. hander：用于执行 `CameraCaptureSession.CaptureCallback` 的 Handler 对象，可以是异步线程的 Handler，也可以是主线程的 Handler，在我们的 Demo 里使用的是主线程 Handler。
```jav
CaptureRequest.Builder requestBuilder = cameraDevice.createCaptureRequest(CameraDevice.TEMPLATE_PREVIEW);
requestBuilder.addTarget(previewSurface);
CaptureRequest request = requestBuilder.build();
captureSession.setRepeatingRequest(request, new RepeatingCaptureStateCallback(), mainHandler);
captureSession.stopRepeating();
```
12. 适配预览比例
```
public class CameraPreview extends TextureView {
    public CameraPreview(Context context) {
        this(context, null);
    }

    public CameraPreview(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public CameraPreview(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int width = MeasureSpec.getSize(widthMeasureSpec);
        setMeasuredDimension(width, width / 3 * 4);
    }
}
```
12. `ImageReader`
	1. 通过` ImageReader.newInstance ()` 方法创建一个 `ImageReader `对象，该方法要求我们传递以下四个参数：
	2. width：图像数据的宽度。
	3. height：图像数据的高度。
	4. format：图像数据的格式，定义在 ` ImageFormat` 里, 有YUV_420_888。
	5. `maxImages`：最大 Image 个数，可以理解成 Image 对象池的大小。
	6. 当有图像数据生成的时候，`ImageReader` 会通过通过` ImageReader.OnImageAvailableListener.onImageAvailable() `方法通知我们，然后我们可以调用 `ImageReader.acquireNextImage() `方法获取存有最新数据的 Image 对象，而在 Image 对象里图像数据又根据不同格式被划分多个部分分别存储在单独的 Plane 对象里，我们可以通过调用 `Image.getPlanes()` 方法获取所有的 Plane 对象的数组，最后通过 `Plane.getBuffer()` 获取每一个 Plane 里存储的图像数据。以 YUV 数据为例，当有 YUV 数据生成的时候，数据会被分成 Y、U、V 三部分分别存储到 Plane 里
```
@Override
public void onImageAvailable(ImageReader imageReader) {
    Image image = imageReader.acquireNextImage();
    if (image != null) {
        Image.Plane[] planes = image.getPlanes();
        Image.Plane yPlane = planes[0];
        Image.Plane uPlane = planes[1];
        Image.Plane vPlane = planes[2];
        ByteBuffer yBuffer = yPlane.getBuffer(); // Data from Y channel
        ByteBuffer uBuffer = uPlane.getBuffer(); // Data from U channel
        ByteBuffer vBuffer = vPlane.getBuffer(); // Data from V channel
    }
    if (image != null) {
        image.close();
    }
}
// 判断YUV_420_888数据是否支持
int imageFormat = ImageFormat.YUV_420_888;
StreamConfigurationMap streamConfigurationMap = cameraCharacteristics.get(CameraCharacteristics.SCALER_STREAM_CONFIGURATION_MAP);
if (streamConfigurationMap != null && streamConfigurationMap.isOutputSupportedFor(imageFormat)) {
    // YUV_420_888 is supported
}

// 获取每一帧预览数据的 ImageReader，并且数据格式为 YUV_420_888
int imageFormat = ImageFormat.YUV_420_888;
StreamConfigurationMap streamConfigurationMap = cameraCharacteristics.get(CameraCharacteristics.SCALER_STREAM_CONFIGURATION_MAP);
if (streamConfigurationMap != null && streamConfigurationMap.isOutputSupportedFor(imageFormat)) {
    previewDataImageReader = ImageReader.newInstance(previewSize.getWidth(), previewSize.getHeight(), imageFormat, 3);
    previewDataImageReader.setOnImageAvailableListener(new OnPreviewDataAvailableListener(), cameraHandler);
    previewDataSurface = previewDataImageReader.getSurface();
}
//创建完 ImageReader，并且获取它的 Surface 之后，我们就可以在创建 Session 的时候添加这个 Surface 告诉 Pipeline 我们有一个专门接收 //YUV_420_888 的 Surface
SessionStateCallback sessionStateCallback = new SessionStateCallback();
List<Surface> outputs = new ArrayList<>();
Surface previewSurface = this.previewSurface;
Surface previewDataSurface = this.previewDataSurface;
outputs.add(previewSurface);
if (previewDataSurface != null) {
    outputs.add(previewDataSurface);
}
try {
    cameraDevice.createCaptureSession(outputs, sessionStateCallback, mainHandler);
} catch (CameraAccessException e) {
    e.printStackTrace();
}

CaptureRequest.Builder requestBuilder = cameraDevice.createCaptureRequest(CameraDevice.TEMPLATE_PREVIEW);
Surface previewSurface = this.previewSurface;
Surface previewDataSurface = this.previewDataSurface;
requestBuilder.addTarget(previewSurface);
if (previewDataSurface != null) {
    requestBuilder.addTarget(previewDataSurface);
}
CaptureRequest request = requestBuilder.build();
captureSession.setRepeatingRequest(request, new RepeatingCaptureStateCallback(), mainHandler);

/**
 * Called every time the preview frame data is available.
 */
@Override
public void onImageAvailable(ImageReader imageReader) {
    Image image = imageReader.acquireNextImage();
    if (image != null) {
        Image.Plane[] planes = image.getPlanes();
        Image.Plane yPlane = planes[0];
        Image.Plane uPlane = planes[1];
        Image.Plane vPlane = planes[2];
        ByteBuffer yBuffer = yPlane.getBuffer(); // Data from Y channel
        ByteBuffer uBuffer = uPlane.getBuffer(); // Data from U channel
        ByteBuffer vBuffer = vPlane.getBuffer(); // Data from V channel
        image.close();
    }
}

```


# 拍摄
1. 单张照片
	1.  拍摄单张照片是最简单的拍照模式，它使用的就是单次模式的 Capture，我们会使用 ImageReader 创建一个接收照片的 Surface，并且把它添加到 CaptureRequest 里提交给相机进行拍照，最后通过 ImageReader 的回调获取 Image 对象，进而获取 JPEG 图像数据进行保存。
```java
//定义回调接口
private val captureResults: BlockingQueue<CaptureResult> = LinkedBlockingDeque()
 
private inner class CaptureImageStateCallback : CameraCaptureSession.CaptureCallback() {
    @MainThread
    override fun onCaptureCompleted(session: CameraCaptureSession, request: CaptureRequest, result: TotalCaptureResult) {
        super.onCaptureCompleted(session, request, result)
        captureResults.put(result)
    }
}
private inner class OnJpegImageAvailableListener : ImageReader.OnImageAvailableListener {
    @WorkerThread
    override fun onImageAvailable(imageReader: ImageReader) {
        val image = imageReader.acquireNextImage()
        val captureResult = captureResults.take()
        if (image != null && captureResult != null) {
            // Save image into sdcard.
        }
    }
}
//创建ImageReader
@WorkerThread
private fun getOptimalSize(cameraCharacteristics: CameraCharacteristics, clazz: Class<*>, maxWidth: Int, maxHeight: Int): Size? {
    val streamConfigurationMap = cameraCharacteristics.get(CameraCharacteristics.SCALER_STREAM_CONFIGURATION_MAP)
    val supportedSizes = streamConfigurationMap?.getOutputSizes(clazz)
    return getOptimalSize(supportedSizes, maxWidth, maxHeight)
}
 
@AnyThread
private fun getOptimalSize(supportedSizes: Array<Size>?, maxWidth: Int, maxHeight: Int): Size? {
    val aspectRatio = maxWidth.toFloat() / maxHeight
    if (supportedSizes != null) {
        for (size in supportedSizes) {
            if (size.width.toFloat() / size.height == aspectRatio && size.height <= maxHeight && size.width <= maxWidth) {
                return size
            }
        }
    }
    return null
}
// 筛选出合适的尺寸，然后创建一个图像格式是 JPEG 的 ImageReader 对象，并且获取它的 Surface：
Size imageSize = getOptimalSize(cameraCharacteristics, ImageReader.class, maxWidth, maxHeight);
if (imageSize != null) {
    jpegImageReader = ImageReader.newInstance(imageSize.getWidth(), imageSize.getHeight(), ImageFormat.JPEG, 5);
    jpegImageReader.setOnImageAvailableListener(new OnJpegImageAvailableListener(), cameraHandler);
    jpegSurface = jpegImageReader.getSurface();
}

// 创建 CaptureRequest
captureImageRequestBuilder = cameraDevice.createCaptureRequest(CameraDevice.TEMPLATE_STILL_CAPTURE);
captureImageRequestBuilder.addTarget(previewDataSurface);
if (jpegSurface != null) {
    captureImageRequestBuilder.addTarget(jpegSurface);
}

// 矫正 JPEG 图片方向
private int getJpegOrientation(CameraCharacteristics cameraCharacteristics, int deviceOrientation) {
    int myDeviceOrientation = deviceOrientation;
    if (myDeviceOrientation == android.view.OrientationEventListener.ORIENTATION_UNKNOWN) {
        return 0;
    }
    Integer sensorOrientation = cameraCharacteristics.get(CameraCharacteristics.SENSOR_ORIENTATION);

    // Round device orientation to a multiple of 90
    myDeviceOrientation = (myDeviceOrientation + 45) / 90 * 90;

    // Reverse device orientation for front-facing cameras
    boolean facingFront = cameraCharacteristics.get(CameraCharacteristics.LENS_FACING) == CameraCharacteristics.LENS_FACING_FRONT;
    if (facingFront) {
        myDeviceOrientation = -myDeviceOrientation;
    }

    // Calculate desired JPEG orientation relative to camera orientation to make
    // the image upright relative to the device orientation
    return (sensorOrientation + myDeviceOrientation + 360) % 360;
}

int deviceOrientation = deviceOrientationListener.getOrientation();
int jpegOrientation = getJpegOrientation(cameraCharacteristics, deviceOrientation);
captureImageRequestBuilder.set(CaptureRequest.JPEG_ORIENTATION, jpegOrientation);


// 设置缩略图尺寸，缩略图尺寸列表是直接通过 CameraCharacteristics.JPEG_AVAILABLE_THUMBNAIL_SIZES 获取的
Size[] availableThumbnailSizes = cameraCharacteristics.get(CameraCharacteristics.JPEG_AVAILABLE_THUMBNAIL_SIZES);
Size thumbnailSize = getOptimalSize(availableThumbnailSizes, maxWidth, maxHeight);

@WorkerThread
private Bitmap getThumbnail(String jpegPath) {
    ExifInterface exifInterface = null;
    try {
        exifInterface = new ExifInterface(jpegPath);
    } catch (IOException e) {
        e.printStackTrace();
        return null;
    }

    int orientationFlag = exifInterface.getAttributeInt(ExifInterface.TAG_ORIENTATION, ExifInterface.ORIENTATION_NORMAL);
    float orientation = 0.0f;
    switch (orientationFlag) {
        case ExifInterface.ORIENTATION_NORMAL:
            orientation = 0.0f;
            break;
        case ExifInterface.ORIENTATION_ROTATE_90:
            orientation = 90.0f;
            break;
        case ExifInterface.ORIENTATION_ROTATE_180:
            orientation = 180.0f;
            break;
        case ExifInterface.ORIENTATION_ROTATE_270:
            orientation = 270.0f;
            break;
        default:
            orientation = 0.0f;
            break;
    }

    Bitmap thumbnail = null;
    if (exifInterface.hasThumbnail()) {
        thumbnail = exifInterface.getThumbnailBitmap();
    } else {
        BitmapFactory.Options options = new BitmapFactory.Options();
        options.inSampleSize = 16;
        thumbnail = BitmapFactory.decodeFile(jpegPath, options);
    }

    if (orientation != 0.0f && thumbnail != null) {
        Matrix matrix = new Matrix();
        matrix.setRotate(orientation);
        thumbnail = Bitmap.createBitmap(thumbnail, 0, 0, thumbnail.getWidth(), thumbnail.getHeight(), matrix, true);
    }

    return thumbnail;
}

//拍照并保存图片
private class OnJpegImageAvailableListener implements ImageReader.OnImageAvailableListener {
 
    private DateFormat dateFormat = new SimpleDateFormat("yyyyMMddHHmmssSSS", Locale.getDefault());
    private String cameraDir = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DCIM) + "/Camera";
 
    @Override
    @WorkerThread
    public void onImageAvailable(ImageReader imageReader) {
        Image image = imageReader.acquireNextImage();
        try {
            CaptureResult captureResult = captureResults.take();
            if (image != null && captureResult != null) {
                ByteBuffer jpegByteBuffer = image.getPlanes()[0].getBuffer(); // Jpeg image data only occupy the planes[0].
                byte[] jpegByteArray = new byte[jpegByteBuffer.remaining()];
                jpegByteBuffer.get(jpegByteArray);
                int width = image.getWidth();
                int height = image.getHeight();
                saveImageExecutor.execute(() -> {
                    long date = System.currentTimeMillis();
                    String title = "IMG_" + dateFormat.format(date); // e.g. IMG_20190211100833786
                    String displayName = title + ".jpeg"; // e.g. IMG_20190211100833786.jpeg
                    String path = cameraDir + "/" + displayName; // e.g. /sdcard/DCIM/Camera/IMG_20190211100833786.jpeg
                    Integer orientation = captureResult.get(CaptureResult.JPEG_ORIENTATION);
                    Location location = captureResult.get(CaptureResult.JPEG_GPS_LOCATION);
                    double longitude = location != null ? location.getLongitude() : 0.0;
                    double latitude = location != null ? location.getLatitude() : 0.0;
 
                    // Write the jpeg data into the specified file.
                    try {
                        FileOutputStream outputStream = new FileOutputStream(path);
                        outputStream.write(jpegByteArray);
                        outputStream.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
 
                    // Insert the image information into the media store.
                    ContentValues values = new ContentValues();
                    values.put(MediaStore.Images.ImageColumns.TITLE, title);
                    values.put(MediaStore.Images.ImageColumns.DISPLAY_NAME, displayName);
                    values.put(MediaStore.Images.ImageColumns.DATA, path);
                    values.put(MediaStore.Images.ImageColumns.DATE_TAKEN, date);
                    values.put(MediaStore.Images.ImageColumns.WIDTH, width);
                    values.put(MediaStore.Images.ImageColumns.HEIGHT, height);
                    values.put(MediaStore.Images.ImageColumns.ORIENTATION, orientation);
                    values.put(MediaStore.Images.ImageColumns.LONGITUDE, longitude);
                    values.put(MediaStore.Images.ImageColumns.LATITUDE, latitude);
                    getContentResolver().insert(MediaStore.Images.Media.EXTERNAL_CONTENT_URI, values);
 
                    // Refresh the thumbnail of image.
                    Bitmap thumbnail = getThumbnail(path);
                    if (thumbnail != null) {
                        runOnUiThread(() -> {
                            thumbnailView.setImageBitmap(thumbnail);
                            thumbnailView.setScaleX(0.8F);
                            thumbnailView.setScaleY(0.8F);
                            thumbnailView.animate().setDuration(50).scaleX(1.0F).scaleY(1.0F).start();
                        });
                    }
                });
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            if (image != null) {
                image.close();
            }
        }
    }
}

//前置摄像头拍照的镜像问题
//解决这个问题的一个办法是拿到 JPEG 数据之后再次对图像进行镜像操作，然后才保存图片。
```
2. 拍摄多张图片
```java
CaptureRequest captureImageRequest = captureImageRequestBuilder.build();
List<CaptureRequest> captureImageRequests = new ArrayList<>();
for (int i = 0; i < burstNumber; i++) {
    captureImageRequests.add(captureImageRequest);
}
captureSession.captureBurst(captureImageRequests, new CaptureImageStateCallback(), mainHandler);
// 后续与单张图片流程相同，每输出一张图片我们就将其保存到 SD 卡并且刷新媒体库和缩略图。

```
3. 切换前后置摄像头
	1. 按照以下顺序进行操作就可以轻松实现前后置摄像头的切换：
	2. 关闭当前摄像头
	3. 开启新的摄像头
	4. 创建新的 Session
	5. 开启预览
```java
@MainThread
private void switchCamera() {
    try {
        CameraDevice cameraDevice = cameraDeviceFuture.get();
        String oldCameraId = cameraDevice != null ? cameraDevice.getId() : null;
        String newCameraId = oldCameraId.equals(frontCameraId) ? backCameraId : frontCameraId;
        if (newCameraId != null) {
            closeCamera();
            openCamera(newCameraId);
            createCaptureRequestBuilders();
            setPreviewSize(MAX_PREVIEW_WIDTH, MAX_PREVIEW_HEIGHT);
            setImageSize(MAX_IMAGE_WIDTH, MAX_IMAGE_HEIGHT);
            createSession();
            startPreview();
        }
    } catch (InterruptedException | ExecutionException e) {
        e.printStackTrace();
    }
}

```
# REF：
1. [一篇文章带你了解Android 最新Camera框架 (qq.com)](https://mp.weixin.qq.com/s?__biz=MzA3ODMzMTM1NA==&mid=2247484350&idx=1&sn=decb3613f95b45d5bf1073373704b2d8&chksm=9f452f0ba832a61d7d77c9d7b492bb5ce9e2785ccacab3473ae1de5c6d40bf5e355c8fad92ca&token=1301012513&lang=zh_CN&scene=21#wechat_redirect)
2. [Camera API2 使用说明-CSDN博客](https://blog.csdn.net/ChaoLi_Chen/article/details/131527858)