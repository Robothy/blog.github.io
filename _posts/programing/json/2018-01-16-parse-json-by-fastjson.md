---
layout: post
categories: programing java json
title: "使用 fastjson 将 JSON 数据与 Java 对象互相转换"
---
# 使用 fastjson 将 JSON 数据与 Java 对象互相转换

fastjson 是阿里巴巴开源的一个 Java 库，其功能完善，性能高，是目前 Java 语言中JSON解析速度最快的库。Github 地址为[https://github.com/alibaba/fastjson](https://github.com/alibaba/fastjson)。[点击这里](https://search.maven.org/remote_content?g=com.alibaba&a=fastjson&v=LATEST "fastjson最新JAR包")下载最新的 JAR 包，或者添加如下 maven dependency 到 pom.xml.

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.44</version>
</dependency>
```

## 将 JSON 数据转化为 Java 对象

在使用 Java 进行开发过程中，经常需要对 JSON 进行解析并将其放到 Java 对象中再进行使用，在也是 fastjson 最常被使用的功能之一。假设需要解析的 JSON 文本如下所示：

```json
{
    "name": "luofuxiang",
    "age": 66
}
```

解析如下Java 类：

``` java
public class Human {

    private String name = null;

    private Integer age = null;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}
```

调用 `JSON.parseObject` 方法即可对数据进行解析：

```java
public static void main(String[] args){
    String jsonString = "{\"name\":\"luofuxiang\",\"age\":66}";
    Human human = JSON.parseObject(jsonString, Human.class);
    System.out.println("Name: " + human.getName());
    System.out.println("Age: " + human.getAge() );
}
```

## 解析 JSON 数组

JSON 数组解析之后存放在 List 当中。

```java
public static void main(String[] args){
    String jsonString = "[{\"name\":\"luofuxiang\",\"age\":66}, {\"name\":\"robothy\",\"age\":65}]";
    List<Human> humans = JSONArray.parseArray(jsonString, Human.class);
    for (Human human : humans){
        System.out.println("Name: " + human.getName());
        System.out.println("Age: " + human.getAge() );
    }
}
```

## 解析复杂结构的 JSON 文本

对于复杂结构的 JSON 文本，针对于文本中出现的所有 JSON 对象，都可以定义与之对应的 Java 类，也可以使用 Java 内部类的方式定义与 JSON 对象对应的类。

### 定义多个类方式

JSON 文本

```json
{
	"human": [{
			"name": "luofuxiang",
			"age": 66
		}, {
			"name": "Baba",
			"age": 32
		}, {
			"name": "Mama",
			"age": 31
		}
	],
	"dog": {
		"name": "Bob",
		"Type": "husky"
	}
}
```

Dog.java

```java
public class Dog {
	
	enum DogType{husky, toy }
	
	String name = null;
	
	DogType type = null;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public DogType getType() {
		return type;
	}

	public void setType(DogType type) {
		this.type = type;
	}
}
```

Family.java

```java
public class Family {
	
	private List<Human> humans = null;
	
	private Dog dog = null;

	public List<Human> getHumans() {
		return humans;
	}

	public void setHumans(List<Human> humans) {
		this.humans = humans;
	}

	public Dog getDog() {
		return dog;
	}

	public void setDog(Dog dog) {
		this.dog = dog;
	}
}
```

```java
String jsonString = "{\"humans\":[{\"name\":\"luofuxiang\",\"age\":66},{\"name\":\"Baba\",\"age\":32},{\"name\":\"Mama\",\"age\":31}],\"dog\":{\"name\":\"Bob\",\"Type\":\"husky\"}}";
Family family = JSON.parseObject(jsonString, Family.class);
```

### 定义内部类方式

Family.java

```java
public class Family {
	
	private List<Human> humans = null;
	
	private Dog dog = null;

	public List<Human> getHumans() {
		return humans;
	}

	public void setHumans(List<Human> humans) {
		this.humans = humans;
	}

	public Dog getDog() {
		return dog;
	}

	public void setDog(Dog dog) {
		this.dog = dog;
	}
	
	class Human {
		
		private String name = null;
		
		private Integer age = null;

		public String getName() {
			return name;
		}

		public void setName(String name) {
			this.name = name;
		}

		public Integer getAge() {
			return age;
		}

		public void setAge(Integer age) {
			this.age = age;
		}
		
	}
	
	class Dog {
		
		enum DogType{husky, toy }
		
		String name = null;
		
		DogType type = null;

		public String getName() {
			return name;
		}

		public void setName(String name) {
			this.name = name;
		}

		public DogType getType() {
			return type;
		}

		public void setType(DogType type) {
			this.type = type;
		}
	}
	
}
```

```java
String jsonString = "{\"humans\":[{\"name\":\"luofuxiang\",\"age\":66},{\"name\":\"Baba\",\"age\":32},{\"name\":\"Mama\",\"age\":31}],\"dog\":{\"name\":\"Bob\",\"Type\":\"husky\"}}";
Family family = JSON.parseObject(jsonString, Family.class);
```