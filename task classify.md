ģ��ֽ�
1.wifi ap��
1)matrix circle_generator(map) //�����Լ����ź�ǿ��-�����,�����һ������,�����ڵ�ͼʲôλ�ø�ap�ź�Ϊ���ٵ���Ϣ
void cicle_generator_test() //���Դ���
{	
	matrix A = circle_generator��map��;
	B = read��"circle for compare.txt"��;//��ȡ��ȷ�ľ���-�ź�ǿ�Ⱦ���
	if (compare(A,B,err) == 1) // �Աȼ���ľ����ʵ�ʾ�����죬�����趨�������err
		printf("test correct\n");
	else 
		printf("test wrong\n");
} 
2��void send_message_to_telephone(map,address,ap_list)//���ֻ��������ݣ���������ݰ�����ͼ��Ϣ��ÿ��ap���ź�ǿ��-�������
���Ա������ֻ��˻���

2.�ֻ���
1�������� data
void recv_message_from_ap()//����ap�˵�����
���Ա�����ap�˻���
void recv_from_user()//���������û������ݣ���Ҫ��Ϊ�˻�ȡĿ�ĵ�
2����λ�� location
coordinate find_my_position(data.map,data.matrix_list��data.interval_time)//�����������������ƶ��Լ�λ��,���һ������
void find_my_position_test()//���Դ���
{
	data mockdata;//����һ��αdata�࣬������������ݽ�������
	coo1 = find_my_postion(mockdaata.map,mockdata.matrix_list,mockdata.interval_time);
	coo2 = read("coordinate for compare.txt")
	if (compare(coo1,coo2,err) == 1)//�Աȹ��ܵõ��Ķ�λ�ͱ�׼��λ
		printf("test correct\n");
	else
		printf("test wrong\n");
}
3��Ѱ·�� path
path find_way_in_same_floor(location.now_coordinate,data.aim)//ͬ��Ѱ·�㷨�����ݵ�ǰλ�ú�Ŀ��λ�ü�������·��
void find_way_in_same_floor_test()//���Դ���
{
	data mockdata;
	location mocklocation;//��������������α��
	path1 = find_way_in_same_floor(mocklocation.now_coordinate,mockdata.aim);
	path2 = read("path for compare.txt");
	if (compare(path1,path2,err) == 1)
		printf("test correct\n");
	else
		printf("test wrong\n"):
}

coordinate find_way_in_different_floor(location.now_coordinate,data.aim)//���Ѱ·�㷨�����ݵ�ǰλ�ú�Ŀ��λ�ã��������ߣ����ص�������¥��λ�ã�ʹ�õĹ��ߣ�
void find_way_in_different_floor_test()//���Դ���
{
	data mockdata;
	location mocklocation;//��������������α��
	coo1 = find_way_in_different_floor(mocklocation.now_coordinate,mockdata.aim);
	coo2 = read("aim_coordinate for compare.txt");
 	if (compare(coo1,coo2,err) == 1)//�Աȹ��ܵõ���Ŀ��λ�úͱ�׼λ��
		printf("test correct\n");
	else
		printf("test wrong\n");
}