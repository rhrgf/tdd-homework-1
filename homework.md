# Wifi室内导航
## 模块分解
### 1. 传感器类`Sensor`
- `List<WifiInfo>& Sensor::getCurrentWifiList()` 调用平台接口，获取当前Wifi列表
- `void Sensot::addWifiInfo(WifiInfo& wifiInfo)` 向Wifi列表中添加信号源信息
- `void Sensor::updateWifiInfo(int wifiId, WifiInfo& wifiInfo)` 更新列表中的`wifiId`指定的Wifi信号源信息，比如位置、信号强度等

### 2. 地图管理器类`MapManager`
- `void MapManager::initMapInfo(MapFile& mapFile)` 给定一个包含室内信息（路线图、障碍物位置及其干扰系数等）的数据文件，将其中的数据存入内存
- `MapInfo& MapManager::getMapInfo()` 获取地图数据信息

### 3. 定位器类`Locator`
- `void Locator::setMapManager(MapManager& mapManager)` 设置定位时需要的地图管理器类
- `void Locator::setSensor(Sensor& sensor)` 设置寻路时需要的传感器类
- `Location& Locator::getCurrentLocation()` 借助设置好的`mapManager`和`sensor`获取当前位置

### 4. 寻路类`PathGenerator`
- `void PathGenerator::setMapManager(MapManager& mapManager)` 设置寻路时需要的地图管理器类
- `Path& PathGerator::getPath(Location& cur, Location& dst)` `cur`是`Locator::getCurrentLocation()`返回的当前位置，`dst`是目标地址. 该函数借助`mapManger`计算一条`cur`到`dst`的路线图

## 测试代码
### 1. 传感器类`Sensor`
```
void TestSensor1()
{
	// new一个Sensor子类，比如 MockSensor
    Sensor* pSensor = new MockSensor;
	if (pSensor->getCurrentWifiList().isEmtry())
    {
    	// 现在列表中还没有任何信息
        print("Test1 passed");
    }
    else
    {
    	print("Test1 failed");
    }
}

void TestSensor2()
{
	pSensor->addWifiInfo(w1);
    pSensor->addWifiInfo(w2);
    pSensor->addWifiInfo(w3);
    pSensor->addWifiInfo(w4);
    
    List<WifiInfo> wifiList = pSensor->getCurrentWifiList();
    
    // 打印 wifiList , 若结果和 addWifiInfo 添加到列表里的一致，则测试通过
}

void TestSensor3()
{
	pSensor->updateWifiInfo(1, w5);
    pSensor->updateWifiInfo(3, w6);
    
    List<WifiInfo>& wifiList = pSensor->getCurrentWifiList();
    
    // 打印 wifiList , 若结果和 updateWifiInfo 更新后的预期结果一致，则测试通过
}

```

### 2. 地图管理器类`MapManager`
```
void TestMapManager1()
{
	// mapFile 中预置了一些测试数据
	mapManager.initMapInfo(mapFile);
    MapInfo mapInfo = mapManger.getMapInfo();
    
    // 检查 mapInfo 和 mapFile，看信息是否吻合
    if (CHECK(mapInfo,mapFile))
    {
    	print("Test1 passed");
    }
    else
    {
    	print("Test1 failed");
    }
}
```

### 3. 定位器类`Locator`
```
void TestLocator()
{
	// mapManager & sensor 根据测试需要手工填充,即直接使用 setter 函数，下同
	locator.setmapManager(mapManager);
    locator.setSensor(sensor);
    Location location = locator.getCurrentLocation();
    // 检查 location 是否符合预期
    // 关于如何模拟一个当前位置？
    // 计算当前位置需要信号源信息和障碍物干扰系数，前者由 sensor.getCurrentWifiList() 提供,
    // 后者由 mapManager.getMapInfo() 提供。那么，可以预置一组模拟的信号源信息和地图数据信息，
    // 使得 getCurrentLocation() 能获得所需数据并计算结果.
}
```

### 4. 寻路类`PathGenerator`
```
void TestPathGenerator()
{
	Location cur, dst; // 两个 Location 结构手工填充
    pathGenerator.setMapManager(mapManger)
    Path path = pathGenerator.getPath(cur, dst);
    // 检查 path 是否符合预期
}
```