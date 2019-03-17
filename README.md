<center><font size = 10>**Manual**</font></center>

# C-style Vector  

## C-style Vector 包含的方法

### 分配新的内存

```
Vector VectorNew(int n);              //整型数组
Vector VectorNew_xy(int x, int y);    //二维整型数组
Vector_D VectorNew_D(int n);          //浮点型数组
Vector_D VectorNew_xy_D(int x, int y);//二维浮点型数组
Vector_Str VectorNew_Str(int n);      //字符串数组,字符串长度30
Vector_Str VectorNew_xy_Str(int x, int y);//二维字符串数组,字符串长度30

```
由以上方法为相应类型的指针分配内存空间.

### 读取与赋值

```
int* VectorGet(Vector *v, int index);
int* VectorGet_xy(Vector *v, int x, int y);
double* VectorGet_D(Vector_D *v, int index);
double* VectorGet_xy_D(Vector_D *v, int x, int y);
char* VectorGet_Str(Vector_Str *v, int index);
char* VectorGet_xy_Str(Vector_Str *v, int x, int y);

```
以上方法用于对已经创建的数组的读取与修改,如果对应指针的内存空间尚未分配或者数组下标
已经超过原本已经分配内存地址范围,以上方法将重新分配内存以适应新的数据规模.但是重新分
配新的内存较为耗时,尽量提前确认数组规模并分配内存空间.

##  C-style Vector 的使用

对于不同的数据类型,内存分配,读取和赋值方法如下:

### 整型

```
#define VGI(xxx,abc) VectorGet(&xxx, abc)
#define VGI2(xxx,aa,bb) VectorGet_xy(&xxx,aa,bb)
...
...
int tmp;
Vector x_int, xy_int;
x_int = VectorNew( n );//n不确定时可以用1代替
tmp = VGI(x_int, i)[i];
VGI(x_int, i)[i] = tmp; //方括号里的i与圆括号里的i应一致

xy_int = VectorNew_xy(x, y);//如果无法确定x,y,可以用1,1代替
tmp = VGI2(xy_int, i, j)[j];
VGI2(xy_int, i, j)[j] = tmp;//方括号里的j与圆括号里的j应一致
```

### 浮点型

```
#define VGD(xxx,abc) VectorGet_D(&xxx, abc)
#define VGD2(xxx,aa,bb) VectorGet_xy_D(&xxx,aa,bb)
...
...
double tmp;
Vector_D x_db, xy_db;
x_db = VectorNew_D( n );//n不确定时可以用1代替
tmp = VGD(x_db, i)[i];
VGD(x_db, i)[i] = tmp; //方括号里的i与圆括号里的i应一致

xy_db = VectorNew_xy_D(x, y);//如果无法确定x,y,可以用1,1代替
tmp = VGD2(xy_db, i, j)[j];
VGD2(xy_db, i, j)[j] = tmp;//方括号里的j与圆括号里的j应一致
```

### 字符串

```
#define VGS(xxx,abc) VectorGet_Str(&xxx, abc)
#define VGS2(xxx,aa,bb) VectorGet_xy_Str(&xxx,aa,bb)
...
...
char tmp[30];
Vector_Str x_str, xy_str;
x_str = VectorNew_Str( n );//n不确定时可以用1代替
strcpy(tmp , VGS(x_str, i));
strcpy(VGS(x_str, i), tmp);

xy_str = VectorNew_xy_Str(x, y);//如果无法确定x,y,可以用1,1代替
strcpy(tmp , VGS2(xy_str, i, j));
strcpy(VGS2(xy_str, i, j), tmp);
```

### 数据存放指针
如果需要获得各种类型数组的头指针,对于整型和浮点型,使用 ***ptr->data*** 即可.

对于字符串数组来说, ***ptr->data*** 为字符串头指针的数组,如需获取某一个字符串的地址,需要
采用格式: ***ptr->data[i]*** 

# 修改rt_model_v10的注意事项

* 通过使用C-style Vector替换掉所有数组,可以实现数据规模的动态可调.

* 为了防止使用并行计算时发生NODE使用的数据还未创建的情况,使用者需要在DEFINE_ON_DEMAND函
数中提前为将要用到的数组分配内存空间.

* 为了防止重复为数组分配内存,请不要重复运行DEFINE_ON_DEMAND函数.

* 为了DEFINE_ADJUST能够运行正确,请添加Massage命令观察程序运行情况,防止因数值问题导
致的错误.
