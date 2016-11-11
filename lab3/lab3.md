##DOL实例分析&编程

@(开发环境)[分布式|DOL|Markdown]
 
- **DOL开发环境配置**
- **分析example** ： 分析example1，example2；
- **分析修改example1** ：使其输出3次方数
- **分析修改example2** ：让3个square模块变成2个:

-------------------

[TOC]

-----------------------
#### DOL开发环境配置

上次实验中我们配置了DOL的开发环境，运行了example1：
```
$	ant -f runexample.xml -Dnumber=1
```
成功结果如图
![Alt text](http://i1.piimg.com/4851/9a075d05a716c5ae.png)

#### 分析example
我们来example文件中各文件夹含义：
##### src文件夹：
- 各进程（生产者，消费者，处理模块等）的功能定义。
- /src 文件夹内包含2种文件：*.c, 与对应的.h，就是实现的模块，就是*.dot的框框的功能描述。（每个模块要实现2个接口，xxx_init和xxx_fire两个函数，分别是初始化这个模块是干了什么，以及这个模块开干的时候做什么
##### example1.xml：
- 系统架构即模块连接方式定义。
- /example*.xml 里面定义了模块与模块之间是怎么连接的，就是有哪些框，哪些线，比如A框跟B框用一根线连起来，他们就在一起了。
- 这个xml是这样的：process就是那些框, sw_channel那些线, connection就是把线的那头连到框的那头。

--------------------------
#### 分析修改example1
运行example1之后的dot图，其中包含生产者、平方模块、消费者
(见博客`http://blog.sina.com.cn/s/blog_7983e5f10100q7jd.html`)
![Alt text](http://i1.piimg.com/4851/2873713acc2057a4.png)
可以看到src里面，对应gennerator这个模块的代码就是gennerator.h, generator.c
![Alt text](http://i1.piimg.com/4851/8ef22a69f16454c2.png)

##### 来看generator.c文件：
```
void generator_init(DOLProcess *p) {
    p->local->index = 0;
    p->local->len = LENGTH;
}
int generator_fire(DOLProcess *p) {
    if (p->local->index < p->local->len) {
        float x = (float)p->local->index;
        //将x写到generator的端口“PORT_OUT”上
        DOL_write((void*)PORT_OUT, &(x), sizeof(float), p);
        p->local->index++;
    }
    if (p->local->index >= p->local->len) {
        DOL_detach(p); //销毁
        return -1;
    }
    return 0;
}
```
* 定义进程：每个模块都要写上xxx_fire（可能被执行无数次），至于init是可选择写或者不写的，xxx_init（只会被执行一次）。
* generator_init 是初始化函数。这里代码的意思是将当前位置置为0，设置生产者长度。这里的local指针指向的是.h文件的_local_states结构。
* generator_fire 是信号产生函数。这里的代码是：如果当前位置小于生产长度，则将x（这里是当前下标）写入到输出端，否则销毁进程。所以说就是，让这个程序被发射、开火、执行length次之后停下来

##### 看consumer.c文件：
```
void consumer_init(DOLProcess *p) {
    sprintf(p->local->name, “consumer”); //就是p->local->name==“consumer”
    p->local->index = 0;
    p->local->len = LENGTH;
}
int consumer_fire(DOLProcess *p) {
    float c;
    if (p->local->index < p->local->len) {
        DOL_read((void*)PORT_IN, &c, sizeof(float), p);//读consumer的端口“PORT_IN”
        printf(“%s: %f\n”, p->local->name, c); //将结构输出到命令行
        p->local->index++;
    }
    if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
    }
    return 0;
}
```
* 定义消费者进程
* consumer_init初始化函数，含义同generator_init。
* consumer_fire信号消费函数，若当前位置小于设定长度，则读出输入端信号，并且打印；否则销毁进程（停下来）
##### 看square.c文件并修改：
```
int square_fire(DOLProcess *p) {
    float i;
    if (p->local->index < p->local->len) {
       // 读square的端口“PORT_IN”,将值读到i
        DOL_read((void*)PORT_IN, &i, sizeof(float), p); 
        i = i*i;     //做了个平方
        // 写square的端口“PORT_OUT”,把 I 写到那个端口
        DOL_write((void*)PORT_OUT, &i, sizeof(float), p); 
        p->local->index++;
    }
    if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
    }
    return 0;
}

```
* 定义平方进程
* square_fire信号处理函数，读入输入端信号i，将其平方后写出到输出端，也是重复length次之后就停止了。
* 将`i=i*i `改为 `i=i*i*i` 就将二次方修改为三次方了。

##### 实验结果：
![Alt text](http://i1.piimg.com/4851/3dca1ece4e18f11a.png)

---------------------------
#### 分析修改example2
各进程功能定义与example1相同，不同之处在于example2架构中中包含3个square进程，故结果为 i^8
![Alt text](http://i1.piimg.com/4851/61b0f00fd3821274.png)
原运行结果：
![Alt text](http://i1.piimg.com/4851/f8aecb4c2eba4bfe.png)
##### 分析修改example2.xml
```
 <variable value="3" name="N"/>
  <!-- instantiate resources -->
  <process name="generator">
    <port type="output" name="10"/>
    <source type="c" location="generator.c"/>
  </process>

  <iterator variable="i" range="N">
    <process name="square">
      <append function="i"/>
      <port type="input" name="0"/>
      <port type="output" name="1"/>
      <source type="c" location="square.c"/>
    </process>
  </iterator>
```
该xml文件中，通过迭代器迭代，定义了三个square模块，因此是(i^2)^3 ，我们将` <variable value="3" name="N"/>`中的“3”改为“2”，再运行看：
![Alt text](http://i1.piimg.com/4851/8077f72895e6ee8d.png)
结果和我们想的一样，变成了两次迭代。
![Alt text](http://i1.piimg.com/4851/600c4a8b52e063fd.png)

###  实验心得

本次实验分析了DOL文件的实例，学习了用graphviz查看dot图，了解了dol生产者、消费者、计算模块之间的关系。
