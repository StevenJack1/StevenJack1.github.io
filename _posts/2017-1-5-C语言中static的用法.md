## static 的用法：  
	#include<iostream>
    using namespace std;

    int fun(){
		static int a = 2;
		a++;	
		cout<<a<<endl;
		return a;
    }
	
    int main(){
		fun();
		fun();
		fun();
		fun();
		fun();
		fun();
		return 0;
    }
### 对于代码段a的值，调用一次fun()都会自增，且值修改后不变，输出结果如下
	3
	4
	5
	6
	7
	8
