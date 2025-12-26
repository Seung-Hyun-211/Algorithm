# Strategy Pattern

알고리즘을 교체하는 디자인 패턴<br>
동일한 목적을 가지는 서로 다른 알고리즘들을 같은 호출로 사용하되 실행중에 교체할 수 있다.


### 실제 사용

아래와 같은 인터페이스를 각각의 다른 알고리즘을 가지는 전략들이 상속받아 오버라이드한다.

```cpp
//전략 인터페이스
class IStrategy
{
public:
    virtual ~IStrategy() = default;
    virtual void Excute() = 0; 
}
```

```cpp
// A, B 패턴
class PatternA : public IStrategy
{
public:
    void Excute() override
    {
        ...
    }
}

class PatternB : public IStrategy
{
public:
    void Excute() override
    {
        ...
    }
}

```

```cpp
//실제 전략 사용
//유니크포인터 -> 인스턴스 재사용하지 않는 구조
class Client
{
private:
    unique_ptr<IStrategy> pattern;

public:
    Client(unique_ptr<IStrategy> st) : IStrategy(move(st))
    {
        ...
    }

    void SetStrategy(unique_ptr<IStrategy> st)
    {
        pattern = move(st);
    }

    void Update()
    {
        pattern->Excute();
    }
}
// 일반 포인터 사용
/*
class Client
{
private:
    IStrategy* pattern;

public:
    Client(IStrategy* st)
    {
        pattern = st;
        ...
    }

    void SetStrategy(IStrategy* st)
    {
        pattern = st;
    }

    void Update()
    {
        pattern->Excute();
    }
}*/
```
