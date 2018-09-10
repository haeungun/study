# Composite Pattern
- 객체들의 관계를 트리 구조로 구성하여 부분-전체 계층을 표현하는 패턴
- 사용자가 단일 객체와 복합 객체 모두 동일하게 다루도록 함
- 예시: 파일/폴더, 파워포인트 도형 그룹화

## Struct
### Component


### Leaf
- 단일 개체

### Composite
- 복합 개체
- 단일 개체와 복합 개체를 자식요소로 포함 가능


## Class Diagram


## Example
````java
// Component
public interfase Shape {
  public void create(int width, int height);
  public void resize(int width, int height);
}

// Leaf
public class Circle implements Shape {
  @Override
  public void create(int width, int height) {
    System.out.println("Created circle: " + width + " * " + height);
  }
  
  @Override
  public void resize(int width, int height) {
    System.out.println("Resized circle: " + width + " * " + height);
  }
}

public class Triangle implements Shape {
  @Override
  public void create(int width, int height) {
    System.out.println("Created triagle: " + width + " * " + height);
  }
  
  @Override
  public void resize(int width, int height) {
    System.out.println("Resized triagle: " + width + " * " + height);
  }
}

// Composite
public class Group implements Shape {
  private List<Shape> shapes;
  
  public Group() {
    this.shapes = new ArrayList<>();
  }
  
  @Override
  public void create(int witdh, int height) {
    for (Shape shape : this.shapes) {
      shape.create(witdh, height);
    }
  }
  
  @Override 
  public void resize(int witdh, int height) {
    for (Shape shape : shapes) {
      shape.resize(width, height);
    }
  }
  
  // helper method
  public void addShape(Shape s) {
    this.shapes.add(s);
  }
  
  // helper method
  public void removeShape(Shape s) {
    this.shapes.remove(s);
  }
  
  // helper method
  public void clear() {
    this.shapes.clear();
  }
}
````
