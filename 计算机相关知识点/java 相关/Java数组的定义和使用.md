## 数组的基本概念

数组本身属于引用数据类型（区别于java的基本数据类型）

数组的定义方式有两种：
1. 声明并开辟数组    数据类型[] 数组名称 = new 数据类型[长度]；

2. 先声明 再开辟     数组类型[] 数组名称(=null);   数组名称=new 数组类型[长度]

```java
//方式一
int[] nums=new nums[10];

//方式二
int[] nums;
nums=new nums[10];
```

数组是引用数据类型，so，数组使用之前一定要实例化，比如 int data[]=null; 然后使用data[2] 就会报错。

获得数组长度的方法: nums.length

## 数组的引用传递

举例：
```java
public class ArrayDemo {
	public static void main(String args[]) {
		int data[] = null;
		data = new int[3]; //开辟一个长度为3的数组
		int temp[] = null; //声明对象
		data[0] = 10;
		data[1] = 20;
		data[2] = 30;
		temp = data;  //int temp[] = data;
		temp[0] = 99;
		for(int i = 0; i < temp.length; i++) {
			System.out.println(data[i]);
		}
	}
}
```

比如上面的代码中，data是在栈空间中定义的一个对象，通过实例化，它指向堆内存中的一片空间，这片空间中存储着一组数组。

那么引用传递，就是上面的代码中，temp=data 这一句，通过这句代码，两个对象同时指向一片区域，所以修改temp[0] 其实就是修改data[0]

引用传递的本质：同一块堆内存空间可以被不同的栈内存所指向。

## 数组静态初始化

上面提到的数组定义的两种方式中，都属于动态的数组初始化（先开辟内存空间，再通过索引赋值），如果想在定义的时候就赋值，就需要静态初始化。

1. 简化格式： 数据类型 数组名称={值，值，……}

2. 完整格式：数据类型 数组名称=new 数据类型[]{值，值，值，……}

举例：
```java
//简化格式
int data={1,2,3};

//完整格式
int data=new int[]{1,2,3};
```

同理，初始化二维数组的方式：
1. 动态初始化：数据类型 数组名称[][]=new 数据类型[行数][列数]；
2. 静态初始化：数据类型 数组名称[][]=new 数据类型[行数][列数]{{值，……}，{值，……}，……}；

举例：
```java
public class ArrayDemo {
	public static void main(String args[]) {
		//此时的数组并不是一个等列数组
		int data[][] = new int[][] {
			{1, 2, 3}, {4, 5}, {6, 7, 8, 9}};
		//如果在进行输出的时候一定要使用双重循环，
		//外部的循环控制输出的行数，而内部的循环控制输出列数
		for(int i = 0; i < data.length; i++) {
			for(int j = 0; j < data[i].length; j++) {
				System.out.print("data[" + i + "][" + j + "]=" + data[i][j] + "、");
			}
			System.out.println();
		}
	}
}
```

## 数组的排序

以冒泡排序为例：

```java
public class ArrayDemo {
    public static void main(String[] args){
        int[] data=new int[]{8,3,6,4,7,5,3,3,1};
        sort(data);
        printArray(data);
    }
    public static void sort(int[] arr){
        for(int i=0;i<arr.length-1;i++){
            for(int j=0;j<arr.length-1-i;j++){
                if(arr[j]>arr[j+1]){
                    int temp=arr[j];
                    arr[j]=arr[j+1];
                    arr[j+1]=temp;
                }
            }
        }
    }
    public static void printArray(int[] arr){
        for(int i=0;i<arr.length;i++){
            System.out.print(arr[i]+"、");
        }
        System.out.println();
    }
}
```

## 数组转置

法一：开辟新的空间
法二：在原数组上两两互换

```java
public class ReverseNums {
    public static void main(String[] args){
        int[] data=new int[]{3,5,7,2,5,2,3};
        //data=reverse(data);
        reverse1(data);
        printArray(data);
    }

    //使用额外空间的做法
    public static int[] reverse(int[] arr){
        int[] temp=new int[arr.length];
        int idx=0;
        for(int i=arr.length-1;i>=0;i--){
            temp[idx++]=arr[i];
        }
        return temp;
    }
    //不使用额外的空间
    public static void reverse1(int arr[]){
        int mid=arr.length/2;
        int head=0,tail=arr.length-1;
        for(int i=0;i<mid;i++){
            int temp=arr[head];
            arr[head]=arr[tail];
            arr[tail]=temp;
            head++;
            tail--;
        }
    }

    public static void printArray(int[] arr){
        for(int i=0;i<arr.length;i++){
            System.out.print(arr[i]+"、");
        }
        System.out.println();
    }
}
```

## 二分查找

```java
public class BinarySearch {
    public static void main(String[] args){
        int data[]=new int[]{1,2,3,4,5};
        int target=3;
        System.out.println(binarysearch(data,target));
    }
    public static int binarysearch(int[] nums,int target){
        int left=0,right=nums.length-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]<target){
                left=mid+1;
            }else if(nums[mid]>target){
                right=mid-1;
            }else{
                return mid;
            }
        }
        return -1;
    }
}
```

## 对象数组

举例，一个Person对象

```java
class Person{
	private int age;
	private String name;
	public Person(String n,int a){
		name=n;
		age=a;
	}
	public String getInfo(){
		return "姓名："+name+"，年龄："+age;
	}
}

//动态初始化
public class ArrayDemo{
	public static void main(String[] args){
		Person per[]=new per[];
		per[0]=new Person("张三",1);
		per[1]=new Person("李四",2);
		per[2]=new Person("王五",3);
		for(int i=0;i<per.length;i++){
			System.out.println(per[i].getInfo());
		}
	}
}
//静态初始化
public class ArrayDemo{
	public static void main(String[] args){
		Person per[]=new Person[]{
		new Person("张三",1),
		new Person("李四",2),
		new Person("王五",3)
		};
		for(int i=0;i<per.length;i++){
			System.out.println(per[i].getInfo());
		}
	}
}
```