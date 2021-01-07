## Observer 패턴

![image](https://user-images.githubusercontent.com/21086676/103884902-9413de00-5122-11eb-92fa-98e5d511a533.png)


- 엑셀을 생각하면 Observer 패턴을 이해하기 쉽다.
- 실제 값이 변할 때, 표/차트/도표에서 값을 반영하려면 어떻게 할까?
    - 과거의 나
         - 각각의 화면에서 while문으로 데이터를 갱신시킨다.
         - 표/차트/도표에서 서로를 각각 알아서, 데이터를 바꾸면 각각의 객체들의 데이터를 갱신시킨다.
    - 현재의 나 > Observer패턴을 쓴다. 
        - 필요한 곳에서 데이터를 Observing하고, 값이 바뀌면 변경된 값을 반영해준다.
        
### 핵심
![image](https://user-images.githubusercontent.com/21086676/103886867-7d22bb00-5125-11eb-83e4-a046097bca39.png)

### 코드
```kotlin
class EventSource {
    private var observers = mutableListOf<Observer>()

    private fun notifyObservers(event: String) {
        observers.forEach { it(event) }
    }

    fun addObserver(observer: Observer) {
        observers += observer
    }

    fun scanSystemIn() {
        val scanner = Scanner(System.`in`)
        while (scanner.hasNext()) {
            val line = scanner.nextLine()
            notifyObservers(line)
        }
    }
}

fun main(arg: List<String>) {
    println("Enter Text: ")
    val eventSource = EventSource()

    eventSource.addObserver { event ->
        println("Received response: $event")
    }

    eventSource.scanSystemIn()
}
```

### 결론
- 데이터 변경이나 이벤트를 받고 싶을 때, 그리고 그 이벤트를 받는 대상이 여러개일 때 사용하면 좋다.
- (주의)사용하지 않는 Observer는 객체의 생명주기에 맞게 제거해줘야 메모리 릭이 안생긴다.

