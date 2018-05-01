模块分解
1.wifi ap端
1)matrix circle_generator(map) //生成自己的信号强度-距离带,输出到一个矩阵,包含在地图什么位置该ap信号为多少的信息
void cicle_generator_test() //测试代码
{	
	matrix A = circle_generator（map）;
	B = read（"circle for compare.txt"）;//读取正确的距离-信号强度矩阵
	if (compare(A,B,err) == 1) // 对比计算的矩阵和实际矩阵差异，可以设定允许误差err
		printf("test correct\n");
	else 
		printf("test wrong\n");
} 
2）void send_message_to_telephone(map,address,ap_list)//与手机传输数据，传输的数据包括地图信息，每个ap的信号强度-距离矩阵
测试必须与手机端互动

2.手机端
1）数据类 data
void recv_message_from_ap()//接收ap端的数据
测试必须与ap端互动
void recv_from_user()//接收来自用户的数据，主要是为了获取目的地
2）定位类 location
coordinate find_my_position(data.map,data.matrix_list，data.interval_time)//根据数据类中数据推断自己位置,输出一个坐标
void find_my_position_test()//测试代码
{
	data mockdata;//定义一个伪data类，里面包含的数据仅作测试
	coo1 = find_my_postion(mockdaata.map,mockdata.matrix_list,mockdata.interval_time);
	coo2 = read("coordinate for compare.txt")
	if (compare(coo1,coo2,err) == 1)//对比功能得到的定位和标准定位
		printf("test correct\n");
	else
		printf("test wrong\n");
}
3）寻路类 path
path find_way_in_same_floor(location.now_coordinate,data.aim)//同层寻路算法，根据当前位置和目标位置计算最优路径
void find_way_in_same_floor_test()//测试代码
{
	data mockdata;
	location mocklocation;//定义两个测试用伪类
	path1 = find_way_in_same_floor(mocklocation.now_coordinate,mockdata.aim);
	path2 = read("path for compare.txt");
	if (compare(path1,path2,err) == 1)
		printf("test correct\n");
	else
		printf("test wrong\n"):
}

coordinate find_way_in_different_floor(location.now_coordinate,data.aim)//异层寻路算法，根据当前位置和目标位置，做出决策，返回的是上下楼的位置（使用的工具）
void find_way_in_different_floor_test()//测试代码
{
	data mockdata;
	location mocklocation;//定义两个测试用伪类
	coo1 = find_way_in_different_floor(mocklocation.now_coordinate,mockdata.aim);
	coo2 = read("aim_coordinate for compare.txt");
 	if (compare(coo1,coo2,err) == 1)//对比功能得到的目标位置和标准位置
		printf("test correct\n");
	else
		printf("test wrong\n");
}