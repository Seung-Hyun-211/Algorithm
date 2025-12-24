# Defendency Injection

### 의존성 주입

하나의 객체가 다른 객체의 의존성을 제공하는 방법.<br>

----
|용어|뜻|
|-|------|
|의존성|서비스로 **사용할 수 있는** 객체|
|주입|의존성을 사용하려는 **객체로** 전달하는 것|
|클라이언트|서비스를 사용(요청)하는 쪽 객체 (사용자)|
|서비스|클라이언트에게 기능을 제공하는 객체 (제공자)|
----

클라이언트가 어떤 서비스(기능)를 사용할 것인지 지정하는 대신,<br>
사용자가 무슨 서비스를 이용할 것인지를 **말하는** 것.

서비스는 클라이언트 상태의 일부이다. 클라이언트가 서비스를 구축하거나 찾는 것을 허용하는 대신<br>
클라이언트에게 서비스를 전달하는 것이 기본 요구조건

### 장점

- 어플리케이션, 클래스가 객체의 생성 방식과 독립적이게 한다.
- 객체의 생성방식을 분리된 구성 파일에서 지정한다.
- 어플리케이션이 다른 구성을 지원한다.

클래스는 객체 생성에 대한 책임이 없어지며, 추상 팩토리처럼 생성을 위임할 필요도 없다.<br>
ㄴ 의문 : 그렇다면 누가 만들고 누가 관리하며, 만들어진 객체는 독립적인 객체인가?<br>
ㄴ 해답 : 자기 자신을 어떻게 생성하는지 몰라도 된다. 객체 본인의 책임(기능)만 담당한다.<br>
ㄴ 생성 : Composition Root 조립지점 에서 모든 객체를 생성하고 한번에 조립"**만**" 하는 유일한 장소를 만든다.

# 예시
``` c#
public interface IServiceA
{
    public string someValue { get; }
    public void DoSomeThing();
}

public class SomeService : IServiceA
{
    public string SomeValue { get; private set; }

    public void DoSomeThing()
    {
        ...
    }
}

public class Client
{
    private readonly IServiceA serviceA;

    public Client(IServiceA serviceA)
    {
        this.serviceA = serviceA;
    }
}


class CompsitionRoot
{
    void Configure()
    {
        IServiceA service = new SomeService();
        Client client = new Client(service); 

        ...

    }

}

```

>![UML](../Image/DI_UML.jpg)
>출처 [Wiki](https://ko.wikipedia.org/wiki/의존성_주입)
