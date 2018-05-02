# wifi室内导航

## 任务描述

通过手机到不同Wifi的强度，计算手机的当前位置。通过选择目标地址，画出路线。当行动时，给出当前位置。

## 任务分配

0. 地图测绘生成地图信息
1. 获取周围不同wifi的强度和位置(传感器)  
2. 通过各个wifi信息计算手机当前位置(定位器)
3. 计算到目标地址的路线算法(路线规划器)
4. UI界面绘制

## 测试驱动开发

### 任务0

1. 地图测绘
```c++
class Map{
    virtual mapImfo * getMapInfoP();
};
```
### 任务1

1. 检测到周围所有wifi
2. 正确的强度信息与MAC地址

```c++

class sensor{
    virtual vector<wifiInfo> getWifiInfoVector(){
        //此函数调用接口实现
    };
};

//use Catch2 for utest
TEST_CASE("Getting wifi vector", [getWifiInfoVector]){
    //get the correct answer
    vector<wifiInfo> realWifiVector = initRealWifiInfoVector();
    //do some test
}
```

### 任务2

1. 根据已知的位置建立信号衰减模型
    1. 信号衰减模型要考虑地形（墙体等）的影响
2. 根据信号衰减模型估算出终端到各个AP的距离
3. 各个AP为中心画等高线设计算法求交点

```c++
//locator 依赖 Map 和 Sensor 所以需要虚拟
class unrealMap: public Map{
    unrealMap(){
        //虚拟一个Map
    }
    mapInfo * getMapInfoP(){
        return unrealMap;
    }
    mapInfo * unrealMap;
};

class unrealSensor: public sensor{
    unrealSensor(){
        //虚拟出所有的信息
    }
    vector<wifiInfo> getWifiInfoVector(){
        return unrealWifiInfoVector;
    };
    vector<wifiInfo> unrealWifiInfoVector;
};

class signalModel{
    signalModel * buildSignalModel(Map * mapModel){
        //...
    }
};

class lacator(){
    vector<distance> getDistanceVector(vector<wifiInfo> & wifiInfoVector){
        //...
    }

    virtual Point locateThePosition(Map * mapModel, vector<wifiInfo> & wifiInfoVector){
        mySignalModel * = new signalModel();
        mySignalModel = buildSignalModel(mapMoodel);

        vector<distance> distanceVector = getDistanceVector(wifiInfoVector);
        //...
    }
};

TEST_CASE("test signalModelBuid"){
    locator ml;
    Map * unrealMap = new uMap();
    MapInfo * map1= unrealMap->getMapInfoP(1);
    signalModel * model1 = initSignalModel(map1);

    REQUIRE(ml.buildSignalModel(map1) = model1);
}

TEST_CASE("test distanceCal"){
    locator ml;
    Map * unrealMap = new uMap();
    mapInfo * map1 = unrealMap->getMapInfoP(1); 
    signalModel * usignalModel = initSignalModel(map1);
    sensor * usensor = new unrealSensor();

    REQUIRE(ml.getDistanceVector(usensor.getWifiInfoVector(), usignalModel) = uDistanceVector);
}

TEST_CASE("test locate"){
    Map * uMap = new unrealMap();
    Sensor * usensor = new unrealSensor();
    REQUIRE(ml.locatePosition(uMap.getMapp(), usensor.getWifiInfoVector()) = realPosition);
}
```

### 任务3

1. 实现点到点的寻路算法输出路径序列
2. 考虑各种障碍物造成的影响

```c++
//路线生成依赖locator所以需要虚拟出定位信息，也依赖map也需要虚拟出map信息。

class unrealLocator:public locator{
    unrealLocator(){
        //生成虚假信息
    }
    Point locateThePosition(Map * mapModel, vector<wifiInfo> & wifiInfoVector){
        return xxx;
    }
};

class route{
    vector<Point> getRoute(Map * mMap, locator * mLocator);
}

TEST_CASE("test route"){
    Locator* uLocator = new unrealLocator();
    Map * umap = new unrealMap();
    route * mroute = new route();
    //REQUIRE
}
```

### 任务4

1. 加载地图
2. 显示wifi信号衰减模型
3. 显示到目标位置最短路径

```c++
//UI 依赖于map, signalModel, route
//虚拟出signalModel, route

class unrealSignalModel: public signalMode{
    unrealSignalModel)(){
        //生成虚假信息
    }
    signalModelInfo * buildSignalModel(Map * mapModel){
        return unrealsignalModelInfo;
    }
    signalModelInfo * unrealsignalModeInfo;
};

class unrealruote : public route{
    unrealroute(){
        //生成虚假信息
    }
    vector<Point> getRoute(Map * mMap, locator * mLocator){
        return unrealPointVector;
    }
    vector<Point> unrealPointVector;
}

TEST_CASE("TEST UI"){
    //do some test
}
```