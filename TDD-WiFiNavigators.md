## WiFi信号获取与距离换算部分
针对此部分进行单独测试：

```Java
// 测试WiFi信号-距离换算类，用假的WiFi传感器
void testWiFiPosCalc () {
    IWiFiSensor mockSensor = new MockWiFiSensor(testenv.mockWiFiData);
    WiFiDistanceCalc pos = new WiFiDistanceCalc(mockSensor);
    ArrayList<WiFiInfo> wifiInfos = pos.GetWiFiInfo();
    AssertEqual(wifiInfos, testenv.expectedWifiInfos);
}
```

根据测试写出源码
```Java
// 接口，也可以理解为策略基类
Interface IWiFiSensor {
    public WiFiInfo GetWiFiInfo();
}
// 用于测试的WiFi传感器
class MockWiFiSensor implements IWiFiSensor{
    private ArrayList<WiFiInfo> wifiInfos;
    public void SetWiFiInfo(WiFiInfos info) { wifiInfos = info; }
    public ArrayList<WiFiInfo> GetWiFiInfo() { return wifiInfo; }
    public MockWiFiSensor(ArrayList<WiFiInfo> infos) { wifiInfos = info; }
}

// 真的WiFi传感器
class WiFiSensor implements IWiFiSensor {
    private WiFiInfo wifiInfo;
    public WiFiInfo GetWiFiInfo() { // 调用系统API拿到数据并返回 };
}
// WiFi场强-距离换算类
class WiFiDistanceCalc {
    private ArrayList<IWiFiSensor> sensors;
    private ArrayList<WiFiInfo> wifiInfo;	// 包含距离，名称信息
   	public void SetWiFiSensor(IWiFiSensor wifiSensor);
    public ArrayList<WiFiInfo> GetWiFiInfo();
}
```

## 地图与寻路算法类

单独测试：

```java
// 测试选路算法，用一张预先准备好的地图，起点终点人为设定，路径也清楚。
void testFindPathStrategy() {
    FindPathStrategy pathFinder = new FindPathStrategy(env.map);	// 这里是mock map
    pathFinder.SetStartPoint(env.x1);
    pathFinder.SetEndPoinr(env.x2);
    ArrayList<Point> route = pathFinder.GetRoute();
    AssertEquals(route, env.expectedRoute);
}
```

需要的类和方法：

```java
class MapInfo extends BaseMapInfo{
	// 存储地图数据，这个必须提前定好
    Point getWiFiPos(String WifiName)
}

class MockMapInfo extends BaseMapInfo{
	// 预先存储了WiFi名称和坐标
}

class FindPathStrategy {
    void SetMap(BaseMapInfo map);
    void SetStartPoint(Point src);
    void SetEndPoint(Point dst);
    ArrayList<Point> GetRoute();	// 输出选好路后的一系列拐点，相邻两个拐点之间是直线
}
```

## 坐标定位部分

单独测试：

```Java
// 测试当前位置定位器，依赖地图数据和WiFi信号-距离换算类，为了构建WiFi信号-距离换算类，需要Mock一堆传感器
void testLocators() {
    MockWiFiPosCalc wifipos = new MockWiFiPosLocator();
    wifipos.set(设置一堆距离和WiFi名)
    ArrayList<WiFiInfo> wifiInfos = wifipos.GetWiFiInfo();
    MockMapInfo mapInfo = new MockMapInfo();
    Locator locator = new Locator;
    for (wifiInfo in wifiInfos) {
        locator.AddWiFiInfo(wifiInfo);
    }
    AssertEquals(locator.GetCurrentPtr(), expectedPoint);
}
```
需要的方法：
```Java
// 尽量不要依赖Map类
class Locator {
    // 可以设定多次，采用在线算法，每调用一次就可以得出一个点
    void AddWiFiInfo(WiFiInfo info);
    // 获取当前点坐标
    Point GetCurrentPtr();
}
```



## 导航器

这里可以测试导航器

```Java
void TestNavigator {
    // 首先组装一个Mock的Locator，传入一堆Mock的Sensor，每隔10秒从MockSensor中读取下一组信息
    IWiFiSensor wifi = new MockWifiSensor(env.testWiFiData);   
    ArrayList<WifiInfo> wifiInfos = wifi.GetWiFiInfo();
    // Mock的Locator，每次GetCurrentPtr()，就切换到下一组数据（数组的下一元素）
    Locator mockLocator = new MockLocator(new WiFiDistanceCalc(wifiInfos));
    // 装配Navigator，传入Listener
    Navigator nav = new Navigator(env.testListener, mockLocator, findPathSrategy);
    nac.setEndPoint(env.EndPoint);
    AssertEqual(testListener.GetRoute, env.FirstRoute());
    //......
   	// 通过改变env.testWiFiData中的数据，即可模演出各种情况，然后在testListener中拿到数据，进而判定    
}


```

对应的导航器需要：

```Java
class Listener {
    // 坐标更新提醒
    public void Update(CurrentPos);
    // 设置信息
   	public void SetInfo(MapInfo, BeginPoint, RouteInfo, CurrentPos);
    // 偏离路线提醒
    public void OnWrongWay(CurrentPos);
    // 返回目的地
    public Point GetDstPos();
}

class Navigator {
    private Point dstPoint;
    private Point beginPoint;
    private Locator locator;
    private MapInfo map;
    private FindPathStrategy findPathStrategy;
    private Listener listener;
    
    private ArrayList<Point> route;
    
    void SetDstPoint(Point dst) {
        dstPoint = dst;
    }
    
    void BeginNav() {
        // 读取地图，根据WiFi信号获取当前坐标
        beginPoint = Locator.GetCurrentPtr();
        // 选路
        findPathStrategy.SetBeginPtr(currentPtr);
        findPathStrategy.SetEndPtr(dstPoint);
        route = FindPathStrategy.GetRoute();
        // 刷新地图UI，开始导航（启动一个线程，定期调用UpdateMap方法）
        listener.SetInfo(MapInfo, BeginPoint, RouteInfo, beginPoint)
        //
        Thread thd = new Thread(this);
        thd1.start();
    }
    
    void run() {
        // 读取地图，根据WiFi信号获取当前坐标
        Point currentPtr = Locator.GetCurrentPtr();
        if (InCurrentRoute(beginPoint, route)) {
            listener.update(beginPoint);
        } else {
            // 提示偏离，重新选路
            listener.NotifyWrongWay(currentPtr);
        	findPathStrategy.SetBeginPtr(currentPtr);
        	findPathStrategy.SetEndPtr(dstPoint);
        	route = FindPathStrategy.GetRoute(currentPtr);
            listener.SetInfo(beginPoint, route， currentPtr);
        }
    }
}
```

## 地图绘制类

```C++
class MapUIDrawer extends Listener{
	private MapInfo map;
	private Point dest;
	private ArrayList<Point> route;

    public void SetDst() {
        // 从UI上得到目的地坐标
    }
    
    public void Update(Point currentPtr) {
        // 刷新地图UI
        DrawCurrentPtr(currentPtr);
        DrawDstPtr(dest);
        DrawRoute();	// 画出路径，已走部分一种颜色，其余部分一种颜色
    }
    
    public void SetInfo(MapInfo, BeginPoint, RouteInfo, CurrentPos) {//...}
}
```
