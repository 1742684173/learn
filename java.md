# 9999.内部类

## 9999.1.静态内部类

```
 	public static void main(String[] args){
        new Inner().print();
    }

    static class Inner{
        public void print() {
            System.out.println("print");
        }
    }
```



## 9999.2.非静态内部类

//注意需要两个new

```
	public static void main(String[] args){
        InnerOne innerOne = new StaticClass().new InnerOne();
        innerOne.print();
    }

    class InnerOne{
        public void print() {
            System.out.println("print");
        }
    }
```

## 9999.3.静态方法内部类

注意：静态方法内部类不能是static

```
 	public static void main(String[] args){
 		printX();
    }

    private static String X = "I am X";
    private static void printX(){
        class Inner{
            public void print(){
                System.out.println(X);
            }
        }

        Inner inner = new Inner();
        inner.print();
    }
```

## 9999.4.非静态方法的内部类

```
	public static void main(String[] args){
 		StaticClass staticClass = new StaticClass();
        staticClass.printX();
    }

    private static String X = "I am X";
    private void printX(){
        class Inner{
            public void print(){
                System.out.println(X);
            }
        }

        Inner inner = new Inner();
        inner.print();
    }
```

## 9999.5.匿名类

直接在需要的时候实例化的类

## 9999.6.内部接口

内部接口一定是static的，因为接口是不能实例化的，所以为了访问到接口中的接口，必须定义为static。如果不指定，则默认就是static。



# 10000.浅拷贝和深拷贝

## 10000.1.浅拷贝

```
public class Address implements Cloneable{
    private String name;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

}
```

```
public class CustUser implements Cloneable{
    private String firstName;
    private String lastName;
    private Address address;
    private String[] cars;

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

}
```

```
Address address = new Address();
        address.setName("北京天安门");

        CustUser custUser = new CustUser();
        custUser.setAddress(address);
        custUser.setFirstName("李");
        custUser.setLastName("磊");
        custUser.setCars(new String[]{"别克","宝马"});

        CustUser custUserCopy=(CustUser) custUser.clone();
        custUserCopy.setFirstName("梅梅");
        custUserCopy.setLastName("韩");
        custUserCopy.getAddress().setName("北京颐和园");
        custUserCopy.getCars()[0]="奥迪";

        log(custUser);
        log(custUserCopy);
```

结果：会影响被拷贝的对象

```
CustUser{firstName='李', lastName='磊', address=Address{name='北京颐和园'}, cars=[奥迪, 宝马]}
CustUser{firstName='梅梅', lastName='韩', address=Address{name='北京颐和园'}, cars=[奥迪, 宝马]}

```

## 10000.2.深拷贝

```
@Override
public Object clone() throws CloneNotSupportedException {
    CustUser custUserDeep = (CustUser)super.clone();
    custUserDeep.address = (Address)address.clone();
    custUserDeep.cars=cars.clone();
    return custUserDeep;
}
```

结果:

```
CustUser{firstName='李', lastName='磊', address=Address{name='北京天安门'}, cars=[别克, 宝马]}
CustUser{firstName='梅梅', lastName='韩', address=Address{name='北京颐和园'}, cars=[奥迪, 宝马]}
```

## 10000.3.不用override clone





## 10000.4.数组拷贝

```
this.cars= Arrays.copyOf(custUserDeep.getCars(),custUserDeep.getCars().length);
```

这样会更快