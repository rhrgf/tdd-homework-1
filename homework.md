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
- `void Locator::setMapManager(MapManager manManager)` 设置寻路时需要的地图管理器类
- `void Locator::setSensor(Sensor& sensor)` 设置寻路时需要的传感器类
- `Path& Locator::getPath()` 获取路线图

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
void TestLocator!()
{
	locator.setmapManager(mapManager);
    locator.setSensor(sensor);
    Path path = locator.getPath(startLocation, endLocation);
    // 检查 path 是否符合预期
}
```