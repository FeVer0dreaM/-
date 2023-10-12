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
