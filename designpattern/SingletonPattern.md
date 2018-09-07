# Singleton Pattern
생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고, 최초 생성된 이후에 호출된 생성자는 최초의 생성자가 생성한 
객체만을 리턴한다. 따라서 싱글턴 패턴은 클래스의 인스턴스가 단 하나임을 보장하고, 이 인스턴스에 접근할 수 있는 방법을 제공한다.
주로 공통된 객체를 여러개 생성해서 사용하는 경우(ex. DBCP)에 많이 사용된다. 
> cf. `python` 은 모듈 그 자체로 싱글턴이다.

## Java Singleton Pattern

### Lazy Singleton
````java
public class Singleton {
  private static Singleton instance;
  
  private Singleton() {}
  
  public static Singleton getInstance() {
    if (instance == null) {
       instance = new Singleton();
    }
    return instance;
  }
}
````
> Multi-thread 환경에서 race condition 발생 가능성이 있기 때문에 바람직하지 않다. 

### Double checked locking Singleton
````java
public class Singleton {
  private static Singleton instance;
  
  private Singleton() {}
  
  public static ThreadSafeSingleton getInstance() {
    if (instance == null) {
      synchronized (ThreadSafeSingleton.class) {
        if (instance == null) {
          instance = new ThreadSafeSingleton();
        }
      }
    }
    return instance;
  }
}
````
> `synchronized` 로 인한 오버헤드를 줄이기 위해 이중으로 인스턴스 체크를 한다.
먼저 진입한 thread 가 Singleton 객체를 메모리에 할당하고 리소스에 대한 참조를 설정했지만, 아직 초기화가 이루어지지 않은 시점에서 
다른 thread 가 `getInstance()` 를 호출하면 잘못된 결과가 반환될 수 있다. 

### Bill Pugh Singleton
````java
public class Singleton {
  private Singleton() {}
  
  public static Singleton getInstance() {
    return SingletonHolder.INSTANCE;
  }
  
  private static class SingletonHolder {
    private static final Singleton INSTANCE = new Singleton();
  }
}
````
> 클래스로더에 의해 참조되는 순간에 동적으로 클래스가 로딩되는 특성을 이용한다.

## Javascript Singleton Pattern
````javascript
const mySingleton = (function() {
   const instance;
   
   function init() {
      // Singleton
      
      // Private members
      const privateVariable = "I'm also private";
      const privateRandomNumber = Math.random();
      
      function privateMethod() {
         console.log("I am private");
      }
      
      return {
         // Public members
         publicMethod: function() {
            console.log("The public can see me!");
         },
         
         publicProperty: "I'm also public",
      
         getRandomNumber: function() {
            return privateRandomNumber;
         }
      };
   };
   
   return {
      getInstance: function() {
         if (!instance) {
            instance = init();
         }
         return instance;
      }
   };
})();

// Usage:
var singleA = mySingleton.getInstance();
var singleB = mySingleton.getInstance();
console.log( singleA.getRandomNumber() === singleB.getRandomNumber() ); // true
````
