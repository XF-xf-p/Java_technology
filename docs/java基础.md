# java基础

## 1.JAVA复制

将一个对象的引用复制给另外一个对象，一共有三种方式。第一种方式是直接赋值，第二种方式是浅拷贝，第三种是深拷贝。所以大家知道了哈，这三种概念实际上都是为了拷贝对象。

### 直接赋值复制

直接赋值。在Java中，A a1 = a2，我们需要理解的是这实际上复制的是引用，也就是说a1和a2指向的是同一个对象。因此，当a1变化的时候，a2里面的成员变量也会跟着变化。

### 浅复制（复制引用但不复制引用的对象）

创建一个新对象，然后将当前对象的非静态字段复制到该新对象，如果字段是值类型的，那么对该字段执行复制；如果该字段是引用类型的话，则复制引用但不复制引用的对象。因此，原始对象及其副本引用同一个对象。

```
class Resume implements Cloneable{  
	public Object clone() {  
		try {  
			return (Resume)super.clone();  
		} catch (Exception e) {  
			e.printStackTrace();  
			return null;  
		}  
	} 
}
```

### 深复制（复制对象和其应用对象）

深拷贝不仅复制对象本身，而且复制对象包含的引用指向的所有对象。

```
class Student implements Cloneable { 
	String name; 
	int age; 
	Professor p; 
	Student(String name, int age, Professor p) { 
		this.name = name; 
		this.age = age; 
		this.p = p; 
	} 
	public Object clone() { 
		Student o = null; 
		try { 
			o = (Student) super.clone(); 
		} catch (CloneNotSupportedException e) { 
			System.out.println(e.toString()); 
		} 
		o.p = (Professor) p.clone(); 
		return o; 
	}
}
```

## 2.序列化（深clone一中实现）

在Java语言里深复制一个对象，常常可以先使对象实现Serializable接口，然后把对象（实际上只是对象的一个拷贝）写到一个流里，再从流里读出来，便可以重建对象。