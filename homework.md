# wifi���ڵ���

## ��������

ͨ���ֻ�����ͬWifi��ǿ�ȣ������ֻ��ĵ�ǰλ�á�ͨ��ѡ��Ŀ���ַ������·�ߡ����ж�ʱ��������ǰλ�á�

## �������

0. ��ͼ������ɵ�ͼ��Ϣ
1. ��ȡ��Χ��ͬwifi��ǿ�Ⱥ�λ��(������)  
2. ͨ������wifi��Ϣ�����ֻ���ǰλ��(��λ��)
3. ���㵽Ŀ���ַ��·���㷨(·�߹滮��)
4. UI�������

## ������������

### ����0

1. ��ͼ���
```c++
class Map{
    virtual mapImfo * getMapInfoP();
};
```
### ����1

1. ��⵽��Χ����wifi
2. ��ȷ��ǿ����Ϣ��MAC��ַ

```c++

class sensor{
    virtual vector<wifiInfo> getWifiInfoVector(){
        //�˺������ýӿ�ʵ��
    };
};

//use Catch2 for utest
TEST_CASE("Getting wifi vector", [getWifiInfoVector]){
    //get the correct answer
    vector<wifiInfo> realWifiVector = initRealWifiInfoVector();
    //do some test
}
```

### ����2

1. ������֪��λ�ý����ź�˥��ģ��
    1. �ź�˥��ģ��Ҫ���ǵ��Σ�ǽ��ȣ���Ӱ��
2. �����ź�˥��ģ�͹�����ն˵�����AP�ľ���
3. ����APΪ���Ļ��ȸ�������㷨�󽻵�

```c++
//locator ���� Map �� Sensor ������Ҫ����
class unrealMap: public Map{
    unrealMap(){
        //����һ��Map
    }
    mapInfo * getMapInfoP(){
        return unrealMap;
    }
    mapInfo * unrealMap;
};

class unrealSensor: public sensor{
    unrealSensor(){
        //��������е���Ϣ
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

### ����3

1. ʵ�ֵ㵽���Ѱ·�㷨���·������
2. ���Ǹ����ϰ�����ɵ�Ӱ��

```c++
//·����������locator������Ҫ�������λ��Ϣ��Ҳ����mapҲ��Ҫ�����map��Ϣ��

class unrealLocator:public locator{
    unrealLocator(){
        //���������Ϣ
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

### ����4

1. ���ص�ͼ
2. ��ʾwifi�ź�˥��ģ��
3. ��ʾ��Ŀ��λ�����·��

```c++
//UI ������map, signalModel, route
//�����signalModel, route

class unrealSignalModel: public signalMode{
    unrealSignalModel)(){
        //���������Ϣ
    }
    signalModelInfo * buildSignalModel(Map * mapModel){
        return unrealsignalModelInfo;
    }
    signalModelInfo * unrealsignalModeInfo;
};

class unrealruote : public route{
    unrealroute(){
        //���������Ϣ
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