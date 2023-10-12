数据结构-顺序表系列操作
1 顺序表基本操作
本题要求实现顺序表元素的增、删、查找以及顺序表输出共4个基本操作函数。L是一个顺序表，函数Status ListInsert_Sq(SqList &L, int pos, ElemType e)是在顺序表的pos位置插入一个元素e（pos应该从1开始），函数Status ListDelete_Sq(SqList &L, int pos, ElemType &e)是删除顺序表的pos位置的元素并用引用型参数e带回（pos应该从1开始），函数int ListLocate_Sq(SqList L, ElemType e)是查询元素e在顺序表的位次并返回（如有多个取第一个位置，返回的是位次，从1开始，不存在则返回0），函数void ListPrint_Sq(SqList L)是输出顺序表元素。实现时需考虑表满扩容的问题。

函数接口定义：
Status ListInsert_Sq(SqList &L, int pos, ElemType e);
Status ListDelete_Sq(SqList &L, int pos, ElemType &e);
int ListLocate_Sq(SqList L, ElemType e);
void ListPrint_Sq(SqList L);
其中 L 是顺序表。 pos 是位置； e 代表元素。当插入与删除操作中的pos参数非法时，函数返回ERROR，否则返回OK。

裁判测试程序样例：

//库函数头文件包含
#include<stdio.h>
#include<malloc.h>
#include<stdlib.h>


//函数状态码定义
#define TRUE        1
#define FALSE       0
#define OK          1
#define ERROR       0
#define INFEASIBLE -1
#define OVERFLOW   -2

typedef int  Status;

//顺序表的存储结构定义
#define LIST_INIT_SIZE  100  
#define LISTINCREMENT   10  //表容量增量
typedef int ElemType;  //假设线性表中的元素均为整型
typedef struct{
    ElemType* elem;   //存储空间基地址
    int length;       //表中元素的个数
    int listsize;     //表容量大小
}SqList;    //顺序表类型定义
Status ListInsert_Sq(SqList &L, int pos, ElemType e);
Status ListDelete_Sq(SqList &L, int pos, ElemType &e);
int ListLocate_Sq(SqList L, ElemType e);
void ListPrint_Sq(SqList L);

//结构初始化与销毁操作
Status InitList_Sq(SqList &L){
  //初始化L为一个空的有序顺序表
    L.elem=(ElemType *)malloc(LIST_INIT_SIZE*sizeof(ElemType));
    if(!L.elem)exit(OVERFLOW);
    L.listsize=LIST_INIT_SIZE;
    L.length=0;
    return OK;
}


int main() {
    SqList L;

    if(InitList_Sq(L)!= OK) {
        printf("InitList_Sq: 初始化失败！！！\n");
        return -1;
    }

    for(int i = 1; i <= 10; ++ i)
        ListInsert_Sq(L, i, i);

    int operationNumber;  //操作次数
    scanf("%d", &operationNumber);

    while(operationNumber != 0) {
        int operationType;  //操作种类
        scanf("%d", & operationType);

        if(operationType == 1) {  //增加操作
            int pos, elem;
            scanf("%d%d", &pos, &elem);
            ListInsert_Sq(L, pos, elem);
        } else if(operationType == 2) {  //删除操作
             int pos; ElemType elem;
             scanf("%d", &pos);
             ListDelete_Sq(L, pos, elem);
             printf("%d\n", elem);
        } else if(operationType == 3) {  //查找定位操作
            ElemType elem;
            scanf("%d", &elem);
            int pos = ListLocate_Sq(L, elem);
            if(pos >= 1 && pos <= L.length)
                printf("%d\n", pos);
            else
                printf("NOT FIND!\n");
        } else if(operationType == 4) {  //输出操作
            ListPrint_Sq(L);
        }
       operationNumber--;
    }
    return 0;
}

/* 请在这里填写答案 */
输入格式：
第一行输入一个整数operationNumber，表示操作数，接下来operationNumber行，每行表示一个操作信息（含“操作种类编号 操作内容”）。 编号为1表示插入操作，后面两个参数表示插入的位置和插入的元素值 编号为2表示删除操作，后面一个参数表示删除的位置 编号为3表示查找操作，后面一个参数表示查找的值 编号为4表示顺序表输出操作
输出格式：
对于操作2,输出删除的元素的值 对于操作3,输出该元素的位置，如果不存在该元素，输出“NOT FOUND”； 对于操作4,顺序输出整个顺序表的元素，两个元素之间用空格隔开，最后一个元素后面没有空格。

输入样例：
4
1 1 11
2 2
3 3
4
输出样例：
1
3
11 2 3 4 5 6 7 8 9 10

2 顺序表创建和就地逆置
本题要求实现顺序表的创建和就地逆置操作函数。L是一个顺序表，函数ListCreate_Sq(SqList &L)用于创建一个顺序表，函数ListReverse_Sq(SqList &L)是在不引入辅助数组的前提下将顺序表中的元素进行逆置，如原顺序表元素依次为1,2,3,4，则逆置后为4,3,2,1。

函数接口定义：
Status ListCreate_Sq(SqList &L);
void ListReverse_Sq(SqList &L);
裁判测试程序样例：
//库函数头文件包含
#include<stdio.h>
#include<malloc.h>
#include<stdlib.h>

//函数状态码定义
#define TRUE        1
#define FALSE       0
#define OK          1
#define ERROR       0
#define INFEASIBLE -1
#define OVERFLOW   -2

typedef int  Status;

//顺序表的存储结构定义
#define LIST_INIT_SIZE  100
#define LISTINCREMENT   10
typedef int ElemType;  //假设线性表中的元素均为整型
typedef struct{
    ElemType* elem;   //存储空间基地址
    int length;       //表中元素的个数
    int listsize;     //表容量大小
}SqList;    //顺序表类型定义

Status ListCreate_Sq(SqList &L);
void ListReverse_Sq(SqList &L);

int main() {
    SqList L;
    ElemType *p;

    if(ListCreate_Sq(L)!= OK) {
        printf("ListCreate_Sq: 创建失败！！！\n");
        return -1;
    }
    
    ListReverse_Sq(L);

    if(L.length){
    for(p=L.elem;p<L.elem+L.length-1;++p){
        printf("%d ",*p);
    }
    printf("%d",*p); 
    }
    return 0;
}
/* 请在这里填写答案 */
输入格式：
第一行输入一个整数n，表示顺序表中元素个数，接下来n个整数为表元素，中间用空格隔开。
输出格式：
输出逆置后顺序表的各个元素，两个元素之间用空格隔开，最后一个元素后面没有空格。

输入样例：
4
1 2 3 4

输出样例：
4 3 2 1

3 有序顺序表的插入
本题要求实现递增顺序表的有序插入函数。L是一个递增的有序顺序表，函数Status ListInsert_SortedSq(SqList &L, ElemType e)用于向顺序表中按递增的顺序插入一个数据。
比如：原数据有：2 5，要插入一个元素3，那么插入后顺序表为2 3 5。
要考虑扩容的问题。

函数接口定义：
Status ListInsert_SortedSq(SqList &L, ElemType e);
裁判测试程序样例：
//库函数头文件包含
#include<stdio.h>
#include<malloc.h>
#include<stdlib.h>

//函数状态码定义
#define TRUE        1
#define FALSE       0
#define OK          1
#define ERROR       0
#define INFEASIBLE -1
#define OVERFLOW   -2

typedef int  Status;

//顺序表的存储结构定义
#define LIST_INIT_SIZE  100
#define LISTINCREMENT   10
typedef int ElemType;  //假设线性表中的元素均为整型
typedef struct{
    ElemType* elem;   //存储空间基地址
    int length;       //表中元素的个数
    int listsize;     //表容量大小
}SqList;    //顺序表类型定义

//函数声明
Status ListInsert_SortedSq(SqList &L, ElemType e);

//顺序表初始化函数
Status InitList_Sq(SqList &L)
{
    //开辟一段空间
    L.elem = (ElemType*)malloc(LIST_INIT_SIZE * sizeof(ElemType));
    //检测开辟是否成功
    if(!L.elem){
        exit(OVERFLOW);
    }
    //赋值
    L.length = 0;
    L.listsize = LIST_INIT_SIZE;

    return OK;
}

//顺序表输出函数
void ListPrint_Sq(SqList L)
{
    ElemType *p = L.elem;//遍历元素用的指针

    
    for(int i = 0; i < L.length; ++i){
        if(i == L.length - 1){
            printf("%d", *(p+i));
        }
        else{
            printf("%d ", *(p+i));
        }
    }
}
int main()
{
    //声明一个顺序表
    SqList L;
    //初始化顺序表
    InitList_Sq(L);

    int number = 0;
    ElemType e;

     scanf("%d", &number);//插入数据的个数 

    for(int i = 0; i < number; ++i)
    {
    scanf("%d", &e);//输入数据
        ListInsert_SortedSq(L, e);
    }

    ListPrint_Sq(L);

    return  0;
}


/* 请在这里填写答案 */
输入格式：
第一行输入接下来要插入的数字的个数
第二行输入数字
输出格式：
输出插入之后的数字

输入样例：
5
2 3 9 8 4

输出样例：
2 3 4 8 9

4 线性表元素的区间删除
给定一个顺序存储的线性表，请设计一个函数删除所有值大于min而且小于max的元素。删除后表中剩余元素保持顺序存储，并且相对位置不能改变。

函数接口定义：
List Delete( List L, ElementType minD, ElementType maxD );
其中List结构定义如下：

typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素在数组中的位置 */
};
L是用户传入的一个线性表，其中ElementType元素可以通过>、==、<进行比较；minD和maxD分别为待删除元素的值域的下、上界。函数Delete应将Data[]中所有值大于minD而且小于maxD的元素删除，同时保证表中剩余元素保持顺序存储，并且相对位置不变，最后返回删除后的表。

裁判测试程序样例：
#include <stdio.h>

#define MAXSIZE 20
typedef int ElementType;

typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};

List ReadInput(); /* 裁判实现，细节不表。元素从下标0开始存储 */
void PrintList( List L ); /* 裁判实现，细节不表 */
List Delete( List L, ElementType minD, ElementType maxD );

int main()
{
    List L;
    ElementType minD, maxD;
    int i;

    L = ReadInput();
    scanf("%d %d", &minD, &maxD);
    L = Delete( L, minD, maxD );
    PrintList( L );

    return 0;
}

/* 你的代码将被嵌在这里 */
输入样例：
10
4 -8 2 12 1 5 9 3 3 10
0 4
输出样例：
4 -8 12 5 9 10 

5 学生信息的那些操作：（6） 删除记录
有一学生成绩表，包括学号、姓名、3门课程成绩。请实现如下删除功能：输入一个学生的学号，删除该学生的所有信息。

输入格式:
首先输入一个整数n(1<=n<=100)，表示学生人数；

然后输入n行，每行包含一个学生的信息：学号（12位）、姓名（不含空格且不超过20位），以及3个整数，表示3门课成绩，数据之间用空格隔开。

最后一行输入一个学号num。

输出格式:
若要删除的学号不存在，则输出error!；否则，输出删除该学生后的所有记录。

输入样例:
在这里给出一组输入。例如：

3
202016040201 Zhangling 78 95 55
202016040202 Wangli 87 99 88
202016040203 Fangfang 68 76 75
202016040201
输出样例:
在这里给出相应的输出。例如：

202016040202 Wangli 87 99 88
202016040203 Fangfang 68 76 75

6 集合减法
给定两个非空集合A和B，集合的元素为30000以内的正整数，编写程序求A-B。

输入格式:
输入为三行。第1行为两个整数n和m，分别为集合A和B包含的元素个数，1≤n, m ≤10000。第2行表示集合A，为n个空格间隔的正整数，每个正整数不超过30000。第3行表示集合B，为m个空格间隔的正整数，每个正整数不超过30000。

输出格式:
输出为一行整数，表示A-B，每个整数后一个空格，各元素按递增顺序输出。若A-B为空集，则输出0，0后无空格。

输入样例:
5 5
1 2 3 4 5
3 4 5 6 7 
输出样例:
1 2 

7 最长连续递增子序列
给定一个顺序存储的线性表，请设计一个算法查找该线性表中最长的连续递增子序列。例如，(1,9,2,5,7,3,4,6,8,0)中最长的递增子序列为(3,4,6,8)。

输入格式:
输入第1行给出正整数n（≤10 
5
 ）；第2行给出n个整数，其间以空格分隔。

输出格式:
在一行中输出第一次出现的最长连续递增子序列，数字之间用空格分隔，序列结尾不能有多余空格。

输入样例：
15
1 9 2 5 7 3 4 6 8 0 11 15 17 17 10
输出样例：
3 4 6 8

