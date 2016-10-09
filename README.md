#DOL������������

@(��������)[�ֲ�ʽ|DOL|Markdown]
 
- **Description ** �� DOL ���������
- **How to install ** ��DOL ��װ�ʼǣ�
- **Experimental experience ** ��ʵ����롢ʵ���ĵ�:

-------------------

[TOC]

## DOL�������

�ֲ�ʽ�����㣨DOL����һ����ʹӦ�ã��룩�Զ�ӳ�䵽�ദ�����ṹƽ̨�Ŀ�ܡ�DOL����������������ɣ�

#### API�ӿڣ�DOL Application Programming Interface��
DOL������һϵ�м���ʹ���ĳ���ӿڣ�ʹ�ֲ�ʽ����ͼ��ƽ̨�ϵĲ���Ӧ�ó�Ϊ�˿��ܡ�����Щ����ӿڣ�Ӧ�ó��򿪷��߿����ڲ���֪���ײ���ϸ�����ݵ�����½��п�������ʵ�ϣ���Щ����ӿ� �ڻ���Ӳ����������� ����һ��ϸ�� ��

#### ģ�⹦�ܣ�DOL Functional Simulation��
Ϊ�˸��������ṩ����Ӧ�õĹ��ܣ�DOL����������ģ�⹦�ܿ�ܡ�����ģ�����Ӧ�ó���֮�⣬����ܹ��������ڻ�ȡӦ�ü����ܲ����ϡ�

#### DOLӳ���Ż���DOL Mapping Optimization��
DOLӳ����Ż�Ŀ������Ҫ����һ���Ӧ�ó���ͼ�νṹƽ̨������ӳ�䡣�ڵ�һ��������XML������Ӧ�úͼ������Ľṹ�Ĺ��Э�� �Ѿ������塣Ȼ��������������ȡ׼ȷ���ܹ��Ƶı�Ҫ��Ϣ�ǰ����ġ�



##  DOL ��װ�ʼ� 

#### Ubuntu�������߼��
 Make���߼��
* ��Linux��Ubuntu�����У�make������Ҫ���������й��̱���ͳ�������
* Makefile�ļ�������make�Ժ��ַ�ʽ����Դ��������ӳ���
* makeͨ���Ƚ϶�Ӧ�ļ��������Ŀ���������������޸�ʱ�䣬��������Щ�ļ���Ҫ���¡���Щ�ļ�����Ҫ���¡�

 Ant���߼��
* Ant��һ�ֻ���Java��build���ߡ�
* Ant��Java��������չ��
* Ant�����������һ�����̽ű����棬�����Զ������ó��������Ŀ�ı��룬��������Եȡ�


#### 1.��װ��Ҫ�Ļ���

 ��װһЩ��Ҫ�Ļ���(ubuntuΪ��)��
```
 $ sudo apt-get update
 $ sudo apt-get install ant
 $ sudo apt-get install openjdk-7-jdk
 $ sudo apt-get install unzip
```

#### 2.�����ļ�
```
sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz

sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip
```
#### 3.��ѹ�ļ�

�½�dol���ļ��� 
```
$	mkdir dol
```
��dolethz.zip��ѹ�� dol�ļ�����
```
$	unzip dol_ethz.zip -d dol
```
��ѹsystemc
```
$	tar -zxvf systemc-2.3.1.tgz
```


#### 4.����systemc
��ѹ�����systemc-2.3.1��Ŀ¼��
```
$	cd systemc-2.3.1
```
�½�һ����ʱ�ļ���objdir
```
$	mkdir objdir
```
������ļ���objdir
```
$	cd objdir
```
����configure(�ܸ���ϵͳ�Ļ�������һ�²��������ڱ���)
```
$	../configure CXX=g++ --disable-async-updates
```
����configure֮��
![Alt text](./1476021111757.png)

���� , ��������ļ�Ŀ¼����

```
$	sudo make install
$ cd ..        $ ls
```
![Alt text](./1476021800061.png)

��¼��ǰ�Ĺ���·��(�������ǰ����·��������������������)
```
$	pwd
```
![Alt text](./1476021907534.png)
�����ʾ�ҵ�ǰ�Ĺ���·��Ϊ /root/systemc-2.3.1

#### 5.����DOL

����ո�dol���ļ���
```
$	cd ../dol
```
�޸�build_zip.xml�ļ�
�ҵ�������λ�������˵��������systemcλ�������
` <property name="systemc.inc" value="YYY/include"/>
<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>`
�� ***YYY*** �ĳ���ҳpwd�Ľ����ע�⣬����  64λ ϵͳ�Ļ�����lib-linuxҪ�ĳ�lib-linux64��

Ȼ���Ǳ��룬���ɹ�����ʾbuild successful
```
$	ant -f build_zip.xml all
```

���ſ����������е�һ�����ӣ�����build/bin/mian·����
```
$	cd build/bin/main
```
Ȼ�����е�һ������
```
$	ant -f runexample.xml -Dnumber=1
```
�ɹ������ͼ
![Alt text](./1476022141077.png)

##  ʵ���ĵ�

����ʵ���˽��������µĹ��ߣ�Markdown��GIt������Markdown��֮ǰ�Ӵ������ı��༭�����Ժ͹�����ȣ�������м���㣬ʹ�÷����ͻ�������ԣ�ѧ��֮���ڽ����ĵ��༭���İ������ȵط����Ժܺõ�Ӧ�á� ��Git���ŵ�Ҳ�Զ��׼�������Ŀ���������������߲��ܴﵽ��Ч�����ڽ���ѧϰ�г���Ӧ�ã���ȡ�ڽ����������Ŀ���õ���Щ����Ĺ��ߡ�
