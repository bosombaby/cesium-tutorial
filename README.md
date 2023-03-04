## 一、前言
![image.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677395310245-9692756d-5aa0-4d13-be4f-8b017633cd97.png#averageHue=%237e8d87&clientId=u64f851b8-2eda-4&from=paste&id=IGosS&name=image.png&originHeight=400&originWidth=600&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=326900&status=done&style=none&taskId=u8815daeb-dc65-4906-ada9-bf0458b2d73&title=)
Cesium是一个用于显示三维地球的开源库，旨在释放3D数据的力量。Cesium基于WebGL技术，能够在Web平台搭建虚拟地球及场景展示应用。

**项目目录**
![11.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677916028780-68fb4d62-1d69-4ff4-a82b-ea9575b876fc.png#averageHue=%2322262d&clientId=uacd0d06a-0f29-4&from=ui&id=ua9ceed37&name=11.png&originHeight=420&originWidth=1110&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=25694&status=done&style=none&taskId=u9f19cc4e-21cb-412d-9eba-207ba029211&title=)

- libs存放一些依赖文件
- stage_0阶段的html页面代码
- stage_1阶段的html页面代码
- token官网的token，记得替换成你的

**开源地址**：Cesium三维地图入门教程
## 二、环境搭建
初始化后的界面及各个控件的名称如图：![image.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677397032052-aaed1160-a874-4e3c-bd6d-540a9d87c735.png#averageHue=%23969d95&clientId=u6f9b8b76-e547-4&from=paste&id=u985028c1&name=image.png&originHeight=750&originWidth=1214&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=878418&status=done&style=none&taskId=ubd9e7157-afe1-449a-93bf-f85ee9ace9b&title=)
```javascript
// 引入token，装入容器
Cesium.Ion.defaultAccessToken='eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI0NmY4OGRkZS03ZTljLTQ5MDMtYmUwZC0wNmM2ZjdmM2M1MzMiLCJpZCI6MTI2MjUzLCJpYXQiOjE2NzczMTI2ODJ9.KO5KCez-xGcJBJfY8XYWAlUXHO4WWrZUm6tCZ1MfCWM'
const viewer=new Cesium.Viewer('cesium-container')
```

cesium主要分为下面四个核心类，后续详细介绍：

- viewer：场景的总管理者
- scene：加载场景的物体，所有3D图形对象的容器
- entity：由Primitive封装而来，主要加载实体模型几何模型
- dataSourceCollection：加载矢量数据
## 三、坐标系及转化
![2.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677397984871-0d828b90-7016-464c-a3cf-72f65deda9f5.png#averageHue=%235d67d1&clientId=u6f9b8b76-e547-4&from=drop&id=u6aaeb138&name=2.png&originHeight=380&originWidth=674&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=160171&status=done&style=none&taskId=u508296ea-ce6b-4562-82a0-b76310f4c37&title=)
### 3.1 wgs84坐标系
![1.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677397945560-5400c20f-37b4-4cd2-9e9f-868d2f2403ef.png#averageHue=%23faf6e6&clientId=u6f9b8b76-e547-4&from=drop&id=u88626e91&name=1.png&originHeight=782&originWidth=1540&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=580234&status=done&style=none&taskId=u2344cba1-4248-4ec7-a9aa-e6db4b0fa45&title=)
[WGS-84坐标系](https://baike.baidu.com/item/WGS-84%E5%9D%90%E6%A0%87%E7%B3%BB/730443?fromModule=lemma_inlink) [1]  的几何意义是：坐标系的原点位于[地球质心](https://baike.baidu.com/item/%E5%9C%B0%E7%90%83%E8%B4%A8%E5%BF%83/7013007?fromModule=lemma_inlink)，z轴指向（[国际时间局](https://baike.baidu.com/item/%E5%9B%BD%E9%99%85%E6%97%B6%E9%97%B4%E5%B1%80/6302772?fromModule=lemma_inlink)）BIH1984.0定义的协议地球极(CTP)方向，x轴指向BIH1984.0的零度[子午面](https://baike.baidu.com/item/%E5%AD%90%E5%8D%88%E9%9D%A2/9688202?fromModule=lemma_inlink)和CTP赤道的交点，y轴通过右手规则确定。
### 3.2 wgs84弧度计算
一般cesium使用这个计算坐标，把经纬度转变为弧度计算

- 根据弧度创建实例：Cesium.Cartographic.fromRadians(longitude, latitude, height, result)
- 经纬度=>弧度：Cesium.Cartographic.fromDegrees(longitude, latitude, height, result)
### 3.3 笛卡尔空间直角坐标系
![1.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677911680677-b445abf8-c6db-4d3c-8db5-79cd1f453c3e.png#averageHue=%23fdfdfd&clientId=uacd0d06a-0f29-4&from=ui&id=ud5a70e67&name=1.png&originHeight=413&originWidth=779&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13477&status=done&style=none&taskId=u45f984f9-6b2a-434f-9884-70733a6dfa9&title=)
**笛卡尔空间直角坐标系**：以地球中心作为原点计算坐标
### 3.4 平面坐标系
![3.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677398402337-6a4fcd37-0449-425e-872a-d70805bb0a2a.png#averageHue=%23f8f8f8&clientId=u6f9b8b76-e547-4&from=ui&id=u3423d764&name=3.png&originHeight=490&originWidth=1236&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=65864&status=done&style=none&taskId=u99d1d6b8-bb20-485f-bace-fefc48698d1&title=)
对于cesium来说通常用第一种坐标系
## 四、视图与场景
###  4.1 Viewer
在Cesium中Viewer是一切的开端，通过**new Cesium.Viewer(container, options)**来创建一个Viewer对象，可以把该对象理解为三维虚拟地球，在Viewer对象上的所有操作，可以看作是对三维虚拟地球的操作。
日常Cesium开发中，几乎都是围绕着这个对象展开的。

![2.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677912173620-c2698793-9663-4182-bb74-ca703d8b2767.png#averageHue=%23fefefe&clientId=uacd0d06a-0f29-4&from=ui&id=u0c5b6c67&name=2.png&originHeight=809&originWidth=1137&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=83485&status=done&style=none&taskId=u26977d79-f9fb-4393-9600-27c61a3cecf&title=)
### 4.2 Scene
Scene为Cesium视图下的3D图形对象和状态的容器，Scene对象并不是显式创建的，而是由Viewer或CesiumWidget初始化视图时隐式创建的，通过Scene对象可以在视图下添加图形（primitive）、添加场景特效（如后处理特效postProcessStage）、添加场景事件或控制视图下的星空skyBox、大气层skyAtmosphere、地球globe、太阳sun和月亮moon。
### 4.3 Camera
在Cesium通过相机来操作场景的视角，从浏览器端看是场景移动，其实是相机移动，所以要注意方向。
**例如**：相机向左移，那么屏幕的场景就会偏右

我们通常可以使用相机的变换完成视角操作：

- 飞行fly
- 缩放zoom
- 移动move
- 视角look
- 平面扭转twist
- 3d旋转rotate
- 将相机视角直接定位到某个位置setView
- 用于将相机视角锁定到目标位置lookAt
- **将地球或场景缩放到该实体的视图范围内viewer.zoomTo()**
## 五、界面操作
### 5.1 视图控件隐藏
```javascript
const viewer = new Cesium.Viewer('cesium-container',{
  timeline:false,
  fullscreenButton: false // 隐藏全屏按钮
}) 


// 隐藏版权信息
viewer._cesiumWidget._creditContainer.style.display = "none"
```

隐藏视图控件可以在创建视图的时候隐藏，如果一开始想要最简化的场景，可以使用下面的代码：
```javascript
// 除了场景，其他控件都被隐藏
const viewer = new Cesium.CesiumWidget("cesiumContainer")
```
### 5.2 场景操作
```javascript
// 场景操作
        // 显示帧率
        viewer.scene.debugShowFramesPerSecond=true
        viewer.scene.skyBox.show=false
        viewer.scene.sun.show = false  // 隐藏太阳
```

场景就是包含一些物体，可以通过上述的方法隐藏
## 六、影像和标注
### 6.1 影像
![4.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677914544541-bd9b06d8-06ec-4637-aa3d-3279d07051dc.png#averageHue=%23314268&clientId=uacd0d06a-0f29-4&from=ui&id=ufba76cba&name=4.png&originHeight=639&originWidth=1146&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=670564&status=done&style=none&taskId=u82760a13-59ca-4ca9-a8ed-b86bc6a028d&title=)

**概述**：所谓的影像就是附着在地球上面的一层贴图，有不同的供应商、创建方式、管理方式，可以叠加。
**加载方式**
```javascript
// 引入token，装入容器
Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI0NmY4OGRkZS03ZTljLTQ5MDMtYmUwZC0wNmM2ZjdmM2M1MzMiLCJpZCI6MTI2MjUzLCJpYXQiOjE2NzczMTI2ODJ9.KO5KCez-xGcJBJfY8XYWAlUXHO4WWrZUm6tCZ1MfCWM'

// 1初始化添加图层，为空默认加载bingmaps
const viewer = new Cesium.Viewer('cesium-container',{
    // imageryProvider:new Cesium.ArcGisMapServerImageryProvider({
    //     url: 'https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer'
    // })
}) 
// 隐藏版权信息
viewer._cesiumWidget._creditContainer.style.display = "none"

//2 后续添加
const  arcGisImagery=new Cesium.ArcGisMapServerImageryProvider({
    url: 'https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer'
})
// viewer.imageryLayers.addImageryProvider(arcGisImagery)


//3更具ionCesium的资源库id添加，需要提前加好资源
const ionNightEarth=new Cesium.IonImageryProvider({assetId:3812})
viewer.imageryLayers.addImageryProvider(ionNightEarth)
```

**管理方式**
```javascript
// 引入token，装入容器
Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI0NmY4OGRkZS03ZTljLTQ5MDMtYmUwZC0wNmM2ZjdmM2M1MzMiLCJpZCI6MTI2MjUzLCJpYXQiOjE2NzczMTI2ODJ9.KO5KCez-xGcJBJfY8XYWAlUXHO4WWrZUm6tCZ1MfCWM'

// 1初始化添加图层
const viewer = new Cesium.Viewer('cesium-container', {
    imageryProvider: new Cesium.ArcGisMapServerImageryProvider({
        url: 'https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer'
    })
})
// 隐藏版权信息
viewer._cesiumWidget._creditContainer.style.display = "none"

//2 后续添加
const arcGisImageryLoad = new Cesium.ArcGisMapServerImageryProvider({
    url: 'https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer'
})
const arcGisImagery = viewer.imageryLayers.addImageryProvider(arcGisImageryLoad)


//3更具ionCesium的资源库id添加，需要提前加好资源
const ionNightEarthLoad= new Cesium.IonImageryProvider({ assetId: 3812 })
const ionNightEarth = viewer.imageryLayers.addImageryProvider(ionNightEarthLoad)

const target = viewer.imageryLayers._layers[1]
target.alpha = 0.5
target.brightness = 2.0
target.contrast = 1.0
// target.hue = 2.0
target.saturation = 1.0
target.gamma=1.0

// ImageryLayerCollection父类，资源导入无法识别，只有viewer.imageryLayers.addImageryProvider才行
const isContains=viewer.imageryLayers.contains(ionNightEarth)
console.log('是否包含',isContains);

const getImagery=viewer.imageryLayers.get(1)
console.log('下标寻找', getImagery);


// 图层移动，lower 1 - 0   raise 0 - 1
viewer.imageryLayers.lowerToBottom(ionNightEarth)


const index = viewer.imageryLayers.indexOf(ionNightEarth)
console.log('下标', index);
```

### 6.2 标注
**概述**：标注也是一层贴图，只不过可以显示具体的位置名称和相关信息

![3.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677914514989-362e2cc4-797c-4208-ad68-5a648a3d8dfd.png#averageHue=%23535e36&clientId=uacd0d06a-0f29-4&from=ui&id=ufc4d6086&name=3.png&originHeight=726&originWidth=1265&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1087362&status=done&style=none&taskId=u4ed040b5-bb88-450c-986a-e75888ed95d&title=)

```javascript
// 引入token，装入容器
Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI0NmY4OGRkZS03ZTljLTQ5MDMtYmUwZC0wNmM2ZjdmM2M1MzMiLCJpZCI6MTI2MjUzLCJpYXQiOjE2NzczMTI2ODJ9.KO5KCez-xGcJBJfY8XYWAlUXHO4WWrZUm6tCZ1MfCWM'


// 1初始化添加图层
const viewer = new Cesium.Viewer('cesium-container', {
  imageryProvider: new Cesium.ArcGisMapServerImageryProvider({
    url: 'https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer'
  })
})
// 隐藏版权信息
viewer._cesiumWidget._creditContainer.style.display = "none"


//  1 高德地图标注
const guadMapLoad=new Cesium.UrlTemplateImageryProvider({
  url: "http://webst02.is.autonavi.com/appmaptile?x={x}&y={y}&z={z}&lang=zh_cn&size=1&scale=1&style=8"
})
// viewer.imageryLayers.addImageryProvider(guadMapLoad)


//  2 天地图标注
const token= '374e3348e10cd75eea1440986e480412'
const webMapTileLoad=new Cesium.WebMapTileServiceImageryProvider({
  url: `http://t0.tianditu.com/cva_w/wmts?service=wmts&request=GetTile&version=1.0.0&LAYER=cva&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&style=default&format=tiles&tk=${token}`,
  layer: "tdtAnnoLayer",
  style: "default",
  format: "image/jpeg",
  tileMatrixSetID: "GoogleMapsCompatible",
  show: false
})
viewer.imageryLayers.addImageryProvider(webMapTileLoad)
```
## 七、地形
Cesium默认加载的地形是没有起伏效果的，和影像加载方式一致。
**外部加载**
```javascript
// 引入token，装入容器
Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI0NmY4OGRkZS03ZTljLTQ5MDMtYmUwZC0wNmM2ZjdmM2M1MzMiLCJpZCI6MTI2MjUzLCJpYXQiOjE2NzczMTI2ODJ9.KO5KCez-xGcJBJfY8XYWAlUXHO4WWrZUm6tCZ1MfCWM'

// 一个视图下地形只能加载一个，而影像图层是可以加载多个的
// 1初始化添加地形
const viewer = new Cesium.Viewer('cesium-container', {
        imageryProvider: new Cesium.ArcGisMapServerImageryProvider({
        url: 'https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer'
    }),
    terrainProvider: new Cesium.ArcGISTiledElevationTerrainProvider({
        url: 'https://elevation3d.arcgis.com/arcgis/rest/services/WorldElevation3D/Terrain3D/ImageServer',
    })
})
// 隐藏版权信息
viewer._cesiumWidget._creditContainer.style.display = "none"

// 2后期添加地形
const ArcGisTerrainProvider = new Cesium.ArcGISTiledElevationTerrainProvider({
        url: 'https://elevation3d.arcgis.com/arcgis/rest/services/WorldElevation3D/Terrain3D/ImageServer',
})
viewer.terrainProvider = ArcGisTerrainProvider
```

**Cesium ion 地形**
![6.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677914865198-fa29a1ed-5a3b-4a49-82d8-493f998424fe.png#averageHue=%2314263a&clientId=uacd0d06a-0f29-4&from=ui&id=ufd5ef2d2&name=6.png&originHeight=901&originWidth=1918&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1779234&status=done&style=none&taskId=ubeca5168-cf77-445e-b4c0-9e87f754fa7&title=)
```javascript
const terrainProvider = Cesium.createWorldTerrain({
        requestWaterMask: true, // 请求水体效果所需要的海岸线数据
})
viewer.terrainProvider = terrainProvider
```
## 八、事件
Cesium中的事件按照类型进行分类，可以分为如下几种：

- 鼠标键盘事件
- 相机事件
- 数据加载事件
- 场景加载事件

## 九、实体
实体（entity）是Cesium中自带的创建图形的方法，通过该方法可以在场景中创建点、线、面、多边形、立方体、圆等基本图形，相当于three.js的物体。

![7.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677915227157-8bf1e132-e6cb-4f8e-9577-b404bf0c60f6.png#averageHue=%23606849&clientId=uacd0d06a-0f29-4&from=ui&id=u9878a7e2&name=7.png&originHeight=885&originWidth=1915&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1959152&status=done&style=none&taskId=ubc0faf55-b9e8-4f71-928a-3d3f6ebc405&title=)

```javascript
const target= Cesium.Cartesian3.fromDegrees(118, 34, 0)
const boxEntity=viewer.entities.add({
    id:'123',
    name:'box-entity',
    position:target,
    box:{
        dimensions:new Cesium.Cartesian3(100,100,100),
        distanceDisplayCondition:new Cesium.DistanceDisplayCondition(0,10000),
        material:Cesium.Color.RED.withAlpha(0.5),
        fill:true,
        outline:true,
        outlineWidth:110,
        outlineColor:Cesium.Color.PINK,
        show:true
    }
})
```
## 十、图形
图形（Primitive）是Cesium中更加高阶的创建图形的方法，那么相对低阶的方法就是使用实体（Entity）定义一个图形。当创建一个图形时，两者的流程都是定义实体的尺寸大小和定义实体的材质外观。图形（Primitive）由两部分组成：

1. 几何形状（Geometry）：定义了Primitive的结构，例如三角形、线条、点等；
2. 外观（Appearance ）：定义Primitive的着色（Sharding），包括GLSL（OpenGL着色语言，OpenGL Shading Language）顶点着色器和片段着色器（ vertex and fragment shaders），以及渲染状态（render state）。

![8.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677915297319-9360466d-a33e-4d35-a251-3164b90f5c8c.png#averageHue=%23799661&clientId=uacd0d06a-0f29-4&from=ui&id=u116c0e8a&name=8.png&originHeight=621&originWidth=997&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=638608&status=done&style=none&taskId=u7e53dd5d-589d-4232-ad55-fba8aaf5b46&title=)
## 十一、模型
### 11.1 gltf
图像库传输格式（Graphic Library Transmission Format, glTF）本质上是一种JSON文件，该文件描述包含以下内容的场景的结构和组成3D模型。
![9.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677915372972-52acc154-836b-483f-aed9-22d5a75bcc65.png#averageHue=%23988c6e&clientId=uacd0d06a-0f29-4&from=ui&id=ue3ab8bed&name=9.png&originHeight=889&originWidth=1917&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1405822&status=done&style=none&taskId=uc53ef340-cd42-4885-848a-1c33030c567&title=)
```javascript
//目标位置与视角方向
const target=Cesium.Cartesian3.fromDegrees(118,34,100)
// 加载物体
const plane=viewer.entities.add({
    name:'flyingPlane',
    position:target,
    
    model:{
        uri:'../libs/Assets/models/CesiumDrone.glb',
        minimumPixelSize: 128,
        maximumScale: 20000,
    }

})
```
### 11.2 3dTitles
3dTiles三维模型使用了 glTF 规范，继承它的渲染高性能，除了嵌入的 glTF，3dTiles 自己 **只记录各级Tile的空间逻辑关系（如何构成整个3dtiles）和属性信息，以及模型与属性如何挂接在一起的信息**
![10.png](https://cdn.nlark.com/yuque/0/2023/png/27367619/1677915385542-6e7286eb-168e-4f01-aaac-2d61d6a24aa6.png#averageHue=%235f5a4b&clientId=uacd0d06a-0f29-4&from=ui&id=ueb7901cb&name=10.png&originHeight=893&originWidth=1920&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1326046&status=done&style=none&taskId=u1dc5f8b0-6e4b-4275-934f-d2939da3f11&title=)
```javascript
//目标位置与视角方向
const tileset= viewer.scene.primitives.add(
    new Cesium.Cesium3DTileset({
        url: '../libs/Assets/3dTitles/tilesset/tileset.json',
    })
)

// 开始调整tileset位置和缩放大小
let params={
    // 模型中心经纬度和高度
    tx:118,
    ty:34,
    tz:0,

    // 模型旋转方向
    rx:0,
    ry:0,
    rz:0,

    // 缩放
    scale:0.5
}

// 显示3dtiles包围盒
tileset.debugShowContentBoundingVolume=true

// 开启 3D Tiles 监视器
// viewer.extend(Cesium.viewerCesium3DTilesInspectorMixin)

// 修改 3D Tiles 的颜色和透明度
// tileset.style = new Cesium.Cesium3DTileStyle({
//     color: "color('rgba(178, 34, 34, 0.5)')", // 淡红色，透明度为0.5，半透明
// })

// 点击修改3dtiles颜色，使用坐标拾取
const handler=new Cesium.ScreenSpaceEventHandler(viewer.scene.canvas)

handler.setInputAction((target)=>{

    // 判断点击的元素
    const feature=viewer.scene.pick(target.position)
    if(feature instanceof Cesium.Cesium3DTileFeature){
        feature.color=Cesium.Color.RED
    }
    // console.log(feature.getProperty('Longitude'));
    console.log(target.position);
},Cesium.ScreenSpaceEventType.LEFT_CLICK)

// 后续修改模型
// 修改高度值大于50的3D Tiles的颜色和透明度
tileset.style = new Cesium.Cesium3DTileStyle({
    color: {
        // 条件筛选
        conditions: [
            ["${Height} > 50", "color('rgba(100,100,100, 0.5)')"],
        ]
    },
    show:'${Height} > 12'
})
```

## **资料**：

- [Cesium中文网](http://cesiumcn.org/)
- [中文网](http://cesium.xin/)
- [Cesium官方](https://www.cesium.com/)
- [Cesium 入门教程](https://syzdev.cn/cesium-docs/)
- [天地图](https://www.tianditu.gov.cn/)
- [Mars3D](http://mars3d.cn/)
