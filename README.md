# 融核软工坊-TDD-课后作业

####################
##计算当前位置

struct point
{
	float x;
	float y;
};//记录WiFi中心点坐标
struct wifiInfo
{
	string name;
	float SignalIndensity;
};//记录WiFi信息

//定义传感器虚接口
class Sensor
{
	virtual list<wifiInfo>
		getCurrentWifiList() = 0;
};

//用传感器数据计算定位
class Locator {
private: Sensor sensor;
public:
	void setSensor(Sensor sensor) {
		this.sensor = sensor;
	}
	point locate() {
		wifis = sensor.getCurrentWifiList();
		/*
		遍历wifis列表，用wifiInfo.name查找数据库获得此wifi信源位置x, y;
		用wifi.singalIntesity换算出到wifi中心的距离dis;
		用wifi的x, wifi的y为圆心，dis为半径形成圆;
		所有的wifi形成的圆相交形成一个相交的区域，选择其中一点作为当前位置curPoint;
		*/
		return curPoint;
	}
};
##测试代码
//模拟传感器数据
class MockSensor : public Sensor {
	List<wifiInfo> list;
	virtual List<wifiInfo> getCurrentWifiList() {
		return list;
	}
	void addInfo(string name, float intensity) {
		wifiInfo info = wifiInfo();
		info.name = name;
		info.SignalIndensity = intensity;
		list.add(info);
	}
};
//用模拟传感器数据测试定位算法
TEST_METHOD(test_Locator)
{
	// TODO: 在此输入测试代码
	MockSensor sensor = MockSensor();
	//模拟三个wifi信源
	sensor.addInfo("wifi1", 0.1);
	sensor.addInfo("wifi2", 0.2);
	sensor.addInfo("wifi3", 0.3);
	Locator locator = new Locator();
	locator.setSensor(sensor);
	point p = locator.locate();
	//检查p是否符合预期;
}



############################
##最优路径算法
struct point
{
	float x;
	float y;
};
struct path_point
{
	point p;
	int node_num;
};//记录路径交点坐标
struct mapInfo
{
	int num;//num是地图中节点个数
	path_point path_points[];
	int path[][];
};//记录地图信息
//当前节点虚接口
class curpoint {
	virtual curpoint <point> getcurpoint() = 0;
};
//虚拟当前地图信息
class curmap {
	virtual map <mapInfo> getcurmap() = 0;
};

//用当前位置和目标位置计算最优路径
class Path {
private: curpoint curnode;
public:
	int nodelist[] = { 0 };//用于保存路径经过的节点号
	path_point path_points[] = { 0 };
	void setcurPoint(curpoint curnode) {
		this.curnode = curnode;
	}
	void insertnode(point aim_point)
	{
		//将当前目标位置当作新的节点插入到图中
		//将目标节点到该路段两端的距离当作两边的权重
	}
	int bestpath(int nodelist[],point curPoint, point aim_point)
	{
		//路径最优算法转化为图论中最小代价算法
		//curpoint->insertnode
		//并保存最优路径经过的节点到nodelist中
		return *nodelist;
	}
};
##测试代码
//模拟当前位置
class Mocknodes :public curpoint {
	curpoint <point> curpoint;
	virtual curpoint<point> getcurpoint() {
		return curpoint;
	}
	void addcurpoint(float x, float y)
	{
		point cur_point = point();
		cur_point.x = x;
		cur_point.y = y;
		curpoint.add(cur_point);
	}
};
//模拟当前地图信息
class Mockmap :public curmap {
	curmap <mapInfo> curmap;
	virtual curmap<mapInfo> getcurmap() {
		return curmap;
	}
	int pathinit()
	{
		curmap.num = 5;
		//具体算法见文档
		//记录路径图中的权重信息
		//以文档中路径图为例
		curmap.path[5][5] = { {0,10,17,20,100},
		{10, 0, 18, 100, 100},
		{17, 18, 0, 21, 100},
		{20, 100, 21, 0, 100 },
		{100, 100, 100, 100, 0} };
		return curmap.num
	}
	void initpath_points(float x,float y,int i)
	{
		mapInfo info = mapInfo();
		curmap->path_points->node_num = i;
		curmap->path_points->p.x = x;
		curmap->path_points->p.y = y;
	}
};

TEST_METHOD(test_path)
{
	Path path1 = new Path;

	//初始化当前点和目标点坐标
	Mocknodes curpoint = Mocknodes();
	Mockmap curmap = Mockmap();
	curpoint.addcurpoint(x1, y1);
	point aim_point = new point;
	aim_point.x = xxx;
	aim_point.y = yyy;
	curmap.pathinit();
	for(int i=0;i<curmap.num;i++)
	{
		curmap.initpath_points(x1,y1,i+1);
	}	
	int nodelist[] = { 0 };

	path1.insertnode(aim_point);
	*nodelist = path1.bestpath(nodelist,curpoint,aim_point);
	//如果返回nodelist的节点是最优路径，则测试成功
}