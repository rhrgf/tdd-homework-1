# WIFI室内导航TDD设计

## 程序员A：系统底层WIFI_API调用处理

### Coding

```
wifi_signalProcess(OSWIFIAPI){
    OSWIFIAPI();// 获得wifi名称，信号大小
    信号大小转换成对应半径值；
    return dict[wifi名，半径]
}
```

### Test

不同测试设备能否正常获得数据（虚拟机设备）

## 程序员B：地图录入和wifi信息整合

### Coding

```
map_Construct(){
    下载或导入地图，转换成对应数据结构；
    下载或导入wifi名称和对应坐标；
    return [map,wifilist]
}
```

### Test

能否正确转换地图及坐标数据

## 程序员C：获得wifi在图上的坐标

### Coding

```
virtual map_Construct;
map_wifiLocQuery(wifi_name){
    wifilist = map_Construct(); // virtual
    return wifilist[wifi_name].location;
}
```

### Test

- 虚拟建图类mock_map_construct extend map_Constuct;
    - 输入不同的图

- 输入拟定的wifi_name,检测输出

## 程序员D：获得定位

### Coding

```
virtual wifi_signalProcess(OSWIFIAPI);
virtual map_wifiLocQuery(i.wifi_name);
loc_getLocation(){
    wifi_info = wifi_signalProcess(OSWIFIAPI);
    for i in wifi_info:
        loc = map_wifiLocQuery(i.wifi_name);
        raw_loc[i] = [loc,i.wifi_radius];
    loc = 根据raw_loc中的wifi坐标和信号半径确定用户位置；
    return loc;
}
```

### Test

- 虚拟信号处理类mock_wifi_signalProcess extend ...
    - 输入拟定的信号wifi名和半径
- 虚拟wifi信号坐标查询类mock_map_wifilocQuery  extend ..
    - 配合输入对应wifi名的坐标
- 检测最后输出的坐标值



## 程序员F ：路径规划

### Coding

```
virtual map_constuct;
route_planner(loc,dst){
    map  = map_Contruct();
    route = 利用最短路算法(map,loc,det);
    return route;
}
```

### Test

- 虚拟mock_map_constuct extend map_constuct;
    - 输入不同的图
- 输入不同的目的地和当前位置数据
- 检测路径规划值是否正确

## 程序员G：导航UI及综合

### Coding

```
virtual loc_getLocation;
viratual route_planner;
UI_navigate(){
    loc = loc_getLocation();
    dest = UI_getdestination(); // 用户输入目的地
    route = route_planner(loc,dst);
    display_route();
}
```

### Test

- 虚拟mock_loc_getLocation extend loc_getLocation
    - 手动输入用户坐标
- 虚拟mock_route_planner extend route_planner
    - 手动对应的目的地和当前位置的路径输入路径
- 检测路径是否显示正确，能否及时纠正偏航