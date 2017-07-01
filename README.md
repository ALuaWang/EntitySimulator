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
g_result = []
def cb(success, models): g_result.append((success, models))

TestTable.objects.select(cb)
g_result[-1][-1]

TestTable(i1 = 12, i2 = 34.0, sm_s1 = "abc", sm_s2 = b"gggtttvvv")
m = TestTable.writeToDB(cb)
m.databaseID

TestTable.objects.filter(databaseID = m.id).update(cb, i1 = 1111, sm_s1 = "cba")
