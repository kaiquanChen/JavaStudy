### 1. 开启四个线程,两个线程对一个变量做+1操作,两个线程对一个变量做-1操作.
>代码如下:
>
    package com.fly.process;
    
    public class MyThread {
    
        public static void main(String[] args) {
            OneThread add = new OneThread("add");
            Thread t1 = new Thread(add);
            Thread t2 = new Thread(add);
    
            OneThread minsu = new OneThread("minsu");
            Thread th1 = new Thread(minsu);
            Thread th2 = new Thread(minsu);
    
            t1.start();
            t2.start();
            th1.start();
            th2.start();
        }
    }
    
    class Singleton {
    
        private static Singleton instance = null;
        private static int number = 0;
    
        private Singleton() {}
    
        public static Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }
    
        public synchronized int add() {
            try {
                System.out.println("执行+1操作前 " + number);
                return ++number;
            } finally {
                System.out.println("执行+1操作后 " + number);
                System.out.println("----------------------");
            }
        }
    
        public synchronized int minus() {
            try {
                System.out.println("执行-1操作前 " + number);
                return --number;
            } finally {
                System.out.println("执行-1操作后 " + number);
                System.out.println("----------------------");
            }
        }
    }
    
    class OneThread implements Runnable {
        private Singleton singleton;
        private String status;
    
        public OneThread(String status) {
            this.singleton = Singleton.getInstance();
            this.status = status;
        }
    
        @Override
        public void run() {
            if ("add".equals(this.status)) {
                System.out.println(singleton.add());
            } else if ("minus".equals(this.status)) {
                System.out.println(singleton.minus());
            }
        }
    }

### 2.编译时与运行时
1. final修饰的字段运行是编译时期计算的,非final修饰的字段都是运行期计算的
2. Java里的泛型是编译时构造的,编译器负责检查程序中类型的正确性,然后把使用了泛型的代码翻译或者重写成可以在当前JVM上的非泛型代码
3. 方法重载是发生在编译时的,也被称为编译时多态,编译器可以根据参数类型来判断
4. 方法重写是发生在运行时的,也被称为运行时多态
5. 注解可以使用在编译期和运行期

