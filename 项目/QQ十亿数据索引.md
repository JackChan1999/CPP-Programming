## QQ十亿数据索引

```C++
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#include<string.h>
#pragma warning(disable:4996)

char path[256] = "E:\\test\\1E~001.txt";
char savepath[256] = "E:\\test\\save2.txt";
char resultpath[256] = "E:\\test\\save3.txt";
//#define lines 84356350
//#define count 10000
//#define restcount 84041869

int lines = 84356350;
int count = 10000;
int restcount = 84040768;

struct qqinfo
{
	int num;
	char passwd[20];
};


//获取有效行数
int getlines()
{
	FILE *pfr = fopen(path, "r");
	if (pfr == NULL)
	{
		printf("File open failed\n");
		return -1;
	}
	else
	{
		int line = 0;
		while (!feof(pfr))
		{
			char buf[200] = { 0 };
			fgets(buf, 200, pfr);
			if (strlen(buf) < 100 && strchr(buf, '-') != NULL)
			{
				line++;
			}
		}
		fclose(pfr);
		return line;
	}
}
//获取密码最大长度
int getmax()
{
	FILE *pfr = fopen(path, "r");
	if (pfr == NULL)
	{
		printf("File open failed\n");
		return -1;
	}
	else
	{
		int max = 0;
		int line = 0;
		int i = 1;
		while (!feof(pfr))
		{
			char buf[200] = { 0 };
			fgets(buf, 200, pfr);
			if (strlen(buf) < 100)
			{
				char *p = strchr(buf, '-');
				if (p != NULL)
				{
					for (int i = 4; i < 24; i++)
					{
						if (p[i]>127 || p[i]<0)
						{
							p[i] = '\0';
							break;
						}
					}
					if (strlen(&p[4]) > max)
					{
						line = i;
						max = strlen(&p[4]);
					}
				}
			}
			i++;
		}
		fclose(pfr);
		printf("最大长度为：%d，所在行为：%d\n", max, line);
		return line;
	}
}

//保存到文件
void saveas()
{
	FILE *pfr = fopen(path, "r");
	FILE *pfw = fopen(savepath, "wb");
	if (pfr == NULL || pfw == NULL)
	{
		printf("File open failed\n");
	}
	else
	{
		struct qqinfo *qqif = (struct qqinfo *)malloc(sizeof(struct qqinfo));
		int linenumber = 0;
		int jishu = 0;
		while (!feof(pfr)&&jishu<lines)
		{
			char buf[256] = { 0 };
			fgets(buf, 256, pfr);
			jishu++;
			if (strlen(buf) < 100)
			{
				char *p = strchr(buf, '-');
				if (p != NULL)
				{
					*p = '\0';
					memset(qqif, 0, sizeof(struct qqinfo));
					qqif->num = atoi(buf);
					//QQ号码规则筛选
					if (qqif->num <= 9999)
					{
						//printf("%d行，%s\n", linenumber, buf);
						continue;
					}
					//密码检测，并恢复
					for (int i = 4; i < 24; i++)
					{
						if (p[i]>127 || p[i]<0)
						{
							p[i] = '\0';
							break;
						}
					}
					if (strlen(&p[4]) >= 6)
					{
						strcpy(qqif->passwd, &p[4]);
						linenumber++;
						fwrite(qqif, sizeof(struct qqinfo), 1, pfw);
					}
				}
			}
		}
		printf("共保存了%d个有效数据\n", linenumber);
		restcount = linenumber;
		free(qqif);
		fclose(pfw);
		fclose(pfr);
	}
}


//打印
void print()
{
	FILE *pfr = fopen(resultpath, "rb");
	if (pfr == NULL)
	{
		printf("print:File open failed\n");
	}
	else
	{
		struct qqinfo *qqif = (struct qqinfo *)malloc(sizeof(struct qqinfo));
		int i = 0;
		while (!feof(pfr) && i < 100 && i < restcount)
		{
			fread(qqif, sizeof(struct qqinfo), 1, pfr);
			printf("%d++++++%s", qqif->num, qqif->passwd);
			i++;
		}
		fclose(pfr);
		free(qqif);
	}
}


//文件希尔排序
void insertDirectByDk(int *p,int *p2 ,int low, int high, int dk)
{
	int temp[2];
	for (int i = low; i < high; i += dk)
	{
		temp[0] = p[i];
		temp[1] = p2[i];
		int j = i - dk;
		for (; j >= low; j -= dk)
		{
			if (temp[0] < p[j])
			{
				p[j + dk] = p[j];
				p2[j + dk] = p2[j];
			}
			else
			{
				break;
			}
		}
		p[j + dk] = temp[0];
		p2[j + dk] = temp[1];
	}
}

void saveto(int *p2)
{
	FILE *pfr = fopen(savepath, "rb");
	FILE *pfw = fopen(resultpath, "wb");
	if (pfr == NULL || pfw == NULL)
	{
		printf("saveto:File open failed\n");
	}
	else
	{
		struct qqinfo temp;
		for (int i = 0; i < restcount; i++)
		{
			fseek(pfr, p2[i] * sizeof(struct qqinfo), SEEK_SET);
			fread(&temp, sizeof(struct qqinfo), 1, pfr);
			fwrite(&temp, sizeof(struct qqinfo), 1, pfw);
			//printf("第%d个:%d======%s   已存\n", i, temp.num, temp.passwd);
			fflush(pfw);
		}
		fclose(pfr);
		fclose(pfw);
	}
}

void myshellSort()
{
	int *qq1 = (int *)malloc(sizeof(int)*restcount);
	int *qq2 = (int *)malloc(sizeof(int)*restcount);
	FILE *pfr = fopen(savepath, "rb");
	if (pfr == NULL)
	{
		printf("文件打开失败\n");
		return;
	}
	else
	{
		struct qqinfo qqif;
		int i = 0;
		//将qq号读取到内存，并记录顺序
		while (!feof(pfr) && i < restcount)
		{
			fread(&qqif, sizeof(struct qqinfo), 1, pfr);
			qq1[i] = qqif.num;
			qq2[i] = i;
			i++;
		}
		fclose(pfr);
		printf("排序读入读取完毕，输入任意键开始排序\n");
		getchar();
		//快速排序
		int len = restcount;
		int dk = len / 2;
		int n = 0;
		while (dk >= 1)
		{
			n++;
			printf("第%d趟排序\n", n);
			for (i = 0; i < dk; i++)
			{
				insertDirectByDk(qq1, qq2, i, restcount, dk);
			}
			dk /= 2;
		}
		printf("排序完毕，输入任意键开始写入硬盘\n");
		getchar();
		saveto(qq2);
	}
}

void search(int qq)
{
	FILE *pfr = fopen(resultpath, "rb");
	int low = 0, high = restcount - 1;
	if (pfr == NULL)
	{
		printf("search:File open failed\n");
		return;
	}
	else
	{
		while (low <= high)
		{
			int mid = (low + high) / 2;
			struct qqinfo qqif;
			fseek(pfr, mid*sizeof(struct qqinfo), SEEK_SET);
			fread(&qqif, sizeof(struct qqinfo), 1, pfr);
			if (qq == qqif.num)
			{
				printf("搜索成功!QQ号：%d QQ密码：%s\n", qqif.num, qqif.passwd);
				break;
			}
			else if (qq < qqif.num)
			{
				high = mid - 1;
			}
			else if (qq>qqif.num)
			{
				low = mid + 1;
			}
		}
		if (low > high)
		{
			printf("没有搜索到该QQ号码\n");
		}
		fclose(pfr);
	}
}

int main()
{
	printf("第一步：找出数据，并将结构体存放硬盘\n");
	saveas();
	printf("存放完毕,按下任意键继续排序");
	getchar();
	myshellSort();
	printf("已存放到硬盘，按下任意键输出前100行\n");
	getchar();
	print();
	getmax();
	while (1)
	{
		int qq = 0;
		scanf("%d", &qq);
		if (qq < 10000)
		{
			break;
		}
		search(qq);
	}

	getchar();
	return 0;
}
```