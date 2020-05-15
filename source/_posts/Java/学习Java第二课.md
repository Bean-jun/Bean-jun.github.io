## 一、基本数据类型
- 整数类型 byte，short,int,long
    - byte:-128~127
    - short:-32768~32767
    - int:-2147483648 ~ 2147483647
    - long: -9223372036854775808 ~ 9223372036854775807
- 浮点数类型 float,double
    - float:3.4x10^38
    - double:1.79x10^308
- 字符类型 char
    - 字符使用`''`表示
    - 字符串使用`""`表示，且需要`String`来表示
- 布尔类型 boolean

## 二、三元运算符
- `b?x:y`
    - 会首先计算`b`，如果`b`为true，则只计算`x`，否则，只计算`y`

## 三、数组类型
- 例如定义一个整型数组
    - `int[] a = new int[3];` 
    - 当然我们可以不用这样写，直接赋值，就不用写数组长度，上述代码非可以写成这样`int[] a = new int[]{11,22,33};`或者这样`int[] a = {11,22,33};`
- 对于一个数组类型，我们可以使用`length`来获取数组的长度
    - a.length; //结果为3

## 四、程序的输入与输出
- 输出（相对输入容易一点）
    - `System.out.println("xxx");`这就是输出语句，当然在输出中我们还可以大做文章
        - 这个时候语句可以写成`System.out.printf("xxx");`注意看`print`变了，后面怎么玩，其实和C语言是差不多相通的
- 输入
    - 导入包`import java.util.Scanner;`
    - 新建一个对象`Scanner scanner = new Scanner(System.in);`
    - 读取输入`int a = scanner.nextInt();`
    - 上述三步才完成了基本的输入，第三部中有很多可以自己定义的地方，比如数据类型

## 五、流程控制
- if else
    - 这个结构其实和Python极为相似，但是不同的是，这里的if后面的语句必须有括号括起来，否则会报错,并且Java的`else if`连用是分开的，在python中可以用`elif`来表示
        - eg 单个if使用
            ```
            int a = 8;
            if (a >= 5) {
                System.out.println("a>5");
            }
            ```
        - eg if和else连用
            ```
            int a = 8;
            if (a >= 15) {
                System.out.println("a>=15");
            }else{
                System.out.println("a<15");
            }
            ```
        - eg if和else if连用
            ```
            int a = 8;
            if (a >= 15) {
                System.out.println("a>=15");
            }else if (a >= 10){
                System.out.println("10<=a<15");
            }else{
                System.out.println("a<10");
            }
            ```
- switch
    - switch其实就是一个多重选择，在给定选项中选择对应数据插入程序，程序执行相应操作
    - 一般语法结构
        ```
        switch (option):
        case option1:
            xxx;
            break;
        case option2:
            xxx;
            break;
        ...
        default:
            xxx;
            break;
        ```
    - 注意上述表示，`case`会枚举`option`中的每一种操作，当然`default`表示，若枚举的都没有，直接执行对应结果就好,关于`break`我们在后面讲解
        - eg
            ```
            import java.util.Scanner;

            public class LearnSwitch {
                public static void main(String[] args) {
                    Scanner num = new Scanner(System.in);
                    int n = num.nextInt();
                    num.close();
                    switch (n) {
                    case 1:
                        System.out.println("1");
                        break;
                    case 2:
                        System.out.println("2");
                        break;
                    default:
                        System.out.println("no in switch!");
                        break;			
                    }
                }
            }
            ```
            该代码，若是输入1或者2就会被`case`捕捉到，执行相应语句，若没有捕捉到，则执行`default`语句。
- while
    - while循环语句
    - 一般语法结构
        ```
        while (条件){
            需要执行的语句
        }
        ```
    - 比如我们执行1+2+3+...+100
        ```
        public class Sum{
            public static void main(String[] args){
                int n = 1;
                int sum = 0;
                while (n<=100){
                    sum += n;
                    n++;
                }
                System.out.println(sum);
            }
        }
        ```
- do while
    - 和while相似，while是先判断再做，do while是先做再判断
    - 一般语法结构
        ```
        do {
            需要执行的语句
        }while (条件);
        ```

- for
    - emmmm,这个就写个代码吧，和c语言基本一致
        ```
        public class Sum {
        public static void main(String[] args) {
            int sum = 0;
            for (int i=1; i<=100; i++) {
                sum = sum + i;
            }
            System.out.println(sum);
            }
        }
        ```
- break && continue
    - 他们两兄弟在循环中用的最多，也是最频繁
    - break会跳出当前循环
    - continue则是提前结束本次循环