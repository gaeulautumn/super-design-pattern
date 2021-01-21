### Template-method-pattern
- 실행해야할 알고리즘의 단계를 정해놓고, subclass에서 구체적인 알고리즘의 내부를 구현하는 패턴
- 상속을 통해 슈퍼클래스의 기능을 확장할 때 사용하는 가장 대표적인 방법.
변하지 않는 기능은 슈퍼클래스에 만들어두고 자주 변경되며 확장할 기능은 서브클래스에서 만들도록 한다.

### Simple UML
<img src = "https://user-images.githubusercontent.com/21086676/105281602-de4f9180-5bef-11eb-97f2-a8acdb490ec5.png" width = 500/>

### Simple UML Code
```java
public abstract class AbstractClass {
    
    protected abstract void hook1();
    
    protected abstract void hook2();
    
    public void templateMethod() {
        hook1();
        hook2();
    }
    
}

public class ConcreteClass extends AbstractClass {

    @Override
    protected void hook1() {
        System.out.println("ABSTRACT hook1 implementation");
    }

    @Override
    protected void hook2() {
        System.out.println("ABSTRACT hook2 implementation");
    }

}

public class TemplateMethodPatternClient {
    public static void main(String[] args) {
        AbstractClass abstractClass = new ConcreteClass();
        abstractClass.templateMethod();
    }
}
```

### Example

```php
{
    abstract protected function initialize();
    abstract protected function startPlay();
    abstract protected function endPlay();

    /** Template method */
    public final function play()
    {
        /** Primitive */
        $this->initialize();

        /** Primitive */
        $this->startPlay();

        /** Primitive */
        $this->endPlay();
    }
}

class Mario extends Game
{
    protected function initialize()
    {
        echo "Mario Game Initialized! Start playing.", PHP_EOL;
    }

    protected function startPlay()
    {
        echo "Mario Game Started. Enjoy the game!", PHP_EOL;
    }

    protected function endPlay()
    {
        echo "Mario Game Finished!", PHP_EOL;
    }

}

class Tankfight extends Game
{
    protected function initialize()
    {
        echo "Tankfight Game Initialized! Start playing.", PHP_EOL;
    }

    protected function startPlay()
    {
        echo "Tankfight Game Started. Enjoy the game!", PHP_EOL;
    }

    protected function endPlay()
    {
        echo "Tankfight Game Finished!", PHP_EOL;
    }

}

$game = new Tankfight();
$game->play();

$game = new Mario();
$game->play();
```
