## Composite Pattern (복합 패턴)
> 개별 객체와 복합 객체를 모두 동일하게 다룰 수 있도록 하는 패턴

#### 상황 (ppt 앱만든다 가정)
<img src = "https://user-images.githubusercontent.com/21086676/102782739-2c009080-43dd-11eb-8624-f6cedf934601.png" width = 400/>

#### 스펙
- 텍스트, 도형, 선등의 기본 요소들이 있어야 한다.
- 몇몇 요소를 묶어서 그룹으로 설정할 수도 있고, 여러개의 그룹을 합쳐서 더 큰 그룹을 만들 수 있어야 한다.

#### 이런 프로그램을 만든다면?
- 텍스트, 도형, 선등의 기본 그림 요소에 대해서 각각 클래스를 정의한다.
- 그룹이라는 클래스를 정의해서 그룹을 어떻게 처리할지 구현한다.

#### 이렇게 되면 문제점은?
- 기본 그림 요소이냐, 그룹이냐에 따라 분기가 필요해진다.
- 기본 그림 요소면 화면에 그린다, 그룹이면 그 안에 있는 요소들을 그린다. 만약 그 안에 그룹이 또 있다면 그룹을 분리해서 처리하는 로직이 필요.

#### Composite Pattern을 사용하면?
![image](https://user-images.githubusercontent.com/21086676/102843563-c64bed00-444c-11eb-8b4b-669c9d884e52.png)

- 별도의 분기를 할 필요가 없이, 그룹이나 기본 그림 요소들을 동일하게 바라보게 됨. (위 그림에선 Graphic)
- 모두 Graphic을 가지고 구현했기 때문에, draw() 함수를 호출하기만 하면 알아서 화면에 그려지게 된다.

#### Composite Pattern
![image](https://user-images.githubusercontent.com/21086676/102843698-1460f080-444d-11eb-8a46-fd67c07c9208.png)

- 위 그림처럼 Tree 구조로 구성되어 있음.

## 코드예제

```java

interface Graphic {
    public void print();
}

class CompositeGraphic implements Graphic {

    private List<Graphic> mChildGraphics = new ArrayList<Graphic>();

    public void print() {
        for (Graphic graphic : mChildGraphics) {
            graphic.print();
        }
    }

    public void add(Graphic graphic) {
        mChildGraphics.add(graphic);
    }

    public void remove(Graphic graphic) {
        mChildGraphics.remove(graphic);
    }
}


/** "Leaf" */
class SingleGraphic implements Graphic {
    public void print() {
        System.out.println("leaf");
    }
}


/** Client */
public class Program {

    public static void main(String[] args) {
        SingleGraphic line = new SingleGraphic();
        SingleGraphic text = new SingleGraphic();
        SingleGraphic shape = new SingleGraphic();

        CompositeGraphic groupGraphics = new CompositeGraphic();
        CompositeGraphic compositeGroupGraphics = new CompositeGraphic();

        groupGraphics.add(line);
        groupGraphics.add(text);
        groupGraphics.add(shape);

        compositeGroupGraphics.add(groupGraphics);
        compositeGroupGraphics.print();
    }
}
```
