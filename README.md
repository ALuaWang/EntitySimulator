EntitySimulator
========================

һ��ģ��Entity��Entity���ݿ����в����Ĺ��ߡ�

���ִ���������������django���κ���django��ص�������ο��������ݣ�
https://docs.djangoproject.com/en/1.11/ref/models/querysets/#queryset-api
https://docs.djangoproject.com/en/1.11/topics/db/queries/#complex-lookups-with-q
https://docs.djangoproject.com/en/1.11/ref/models/expressions/#django.db.models.F



��װ������
---------------------
	1.����commonĿ¼���������ݲ����Ƶ�KBEngine����ġ�kbe\res\scripts\common\�����棨��Ȼ�����Ƶ�����ű�������Ҳ���ԣ���
	2.����MysqlUtility.py�õ���Mysql-Connector��һЩ�����������Ҫ��https://cdn.mysql.com//Downloads/Connector-Python/mysql-connector-python-2.1.6.zip������python�汾��mysql-connector��
	3.��ѹ��mysql-connector-python-2.1.6.zip�����ơ�mysql-connector-python-2.1.6\lib\mysql��Ŀ¼��KBEngine����ġ�kbe\res\scripts\common\�����档
	4.over.



���Դ��룺
---------------------
	from EntitySimulator.EntityModel import EntityModel
	from EntitySimulator import Fields

	class TestTable( EntityModel ):
		class Meta:
			db_table = "custom_TestTable"

		databaseID = Fields.INT32( db_column = "id", primary_key = True )
		i1         = Fields.INT32( db_column = "sm_i1" )
		i2         = Fields.FLOAT( db_column = "sm_i2" )
		sm_s1      = Fields.UNICODE(default = "test1")
		sm_s2      = Fields.BLOB(default = b'gdsadf')



telnet localhost 40000
---------------------
	from EntitySimulator.utils.query_utils import Q
	from EntitySimulator.utils.expressions import F

	g_result = []
	def cb(success, models): g_result.append((success, models))

	TestTable.objects.select(cb)

	# ����
	TestTable.objects.select(cb, i1 = 1)

	# ����
	TestTable.objects.select(cb, i1__gt = 1)

	# ���ڵ���
	TestTable.objects.select(cb, i1__gte = 1)

	# С��
	TestTable.objects.select(cb, i1__lt = 1)

	# С�ڵ���
	TestTable.objects.select(cb, i1__lte = 1)

	# ֵ��1��3��7
	TestTable.objects.select(cb, i1__in = (1,3,7))

	# �ڷ�Χ 1 - 10 ֮��
	TestTable.objects.select(cb, i1__range = (1, 10))

	# xxx ILIKE 'yyy'
	TestTable.objects.select(cb, sm_s1__iexact = "abc")
	TestTable.objects.select(cb, sm_s1__iexact = None)

	# С�ڵ���10����ڵ���100
	TestTable.objects.select(cb, Q(i1__lte = 10) | Q(i2__gte = 100))

	# order by �Լ� limit
	TestTable.objects.order_by("databaseID", "-i1").limit(1, 10).select(cb, databaseID = 100)

	TestTable.objects.filter(databaseID = 1).update(cb, i1 = 1111, sm_s1 = "cba")
	TestTable.objects.filter(databaseID = 123).update(cb, i1 = 220 + F("i1") + 110, sm_s1 = "cba")
	TestTable.objects.filter(databaseID = 456).update(cb, i1 = 220 + F("i1") + F("i2") + 110, sm_s1 = "cba")


	# д������
	m = TestTable(i1 = 12, i2 = 34.0, sm_s1 = "abc", sm_s2 = b"gggtttvvv")
	m.writeToDB(cb)
	m.databaseID

	TestTable.objects.filter(databaseID = m.id).update(cb, i1 = 1111, sm_s1 = "cba")
	TestTable.objects.filter(databaseID = m.id).update(cb, i1 = F("i1") + 110, sm_s1 = "cba")

	# update custom_TestTable set sm_i1 = 220 + sm_i1 + sm_i2 + 110, sm_s1 = "cha" where id = xxx
	TestTable.objects.filter(databaseID = m.id).update(cb, i1 = 220 + F("i1") + F("i2") + 110, sm_s1 = "cba")
