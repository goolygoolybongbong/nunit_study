# NUnit 소개

NUnit은 .Net언어(C#, F#, VisualBasic)를 위한 단위 테스트 프레임워크입니다. JUnit(Java의 대표적인 테스트 프레임 워크)에서 영향을 받아 만들어졌으며, .Net 환경에서 자동화 테스트를 지원하는 데 널리 사용됩니다.


# AAA 패턴으로 NUnit 테스트 작성하기

AAA 패턴은 **Arange, Act, Assert**의 약자로, 주로 단위 테스트에서 사용되는 테스트 작성 패턴입니다. 이 패턴은 테스트 코드를 작성할 때 테스트가 명확하고 일관성 있게 작성될 수 있도록 도와줍니다.

1. **Arrange(준비)**: 테스트할 환경을 설정하는 단계. 테스트 대상이 되는 객체를 생성하거나 초기화하고, 필요한 데이터나 상태를 준비 
2. **Act(실행)**: 실제로 테스트하려는 동작을 수행하는 단계. 함수를 호출하고 준비된 데이터나 객체 전달하며 출력 값을 캡처한다.
3. **Assert(검증)**: 수행된 동작이 예상한 결과와 일치하는지 확인하는 단계. 테스트 성공여부를 판단. 결과는 반환 값, 상태 등으로 표시될 수 있다.

예를 들어 아래와 같은 Calculator 클래스가 있다고 하고 두 숫자의 합을 계산하는 `Sum함수`를 검증하고자 한다.
 
```C#
public class Calculator
{
    public double Sum(double a, double b)
    {
        return a + b;
    }
}
```

AAA패턴을 적용한 검증은 아래의 코드와 같다.

```C#
[TestFixture]
public class CalculatorTests
{
    [Test]
    public void Sum_of_two_numbers()
    {
        // Arrange
        double first = 10;
        double second = 20;
        var calculator = new Calculator();

        // Act
        double result = calculator.Sum(first, second);

        // Assert
        Assert.That(result, Is.EqualTo(30));
    }
}
```

* `[TestFixture]`: NUnit에서 테스트 클래스를 정의할 때 사용하는 어트리뷰트. 이 어트리뷰트가 붙은 클래스는 **테스트 대상 클래스**라는 것을 NUnit에게 알려주는 역할을 함

* `[Test]`: 실제 테스트 메서드임을 나타내는 어트리뷰트. 이 어트리뷰트가 붙은 메서드는 **테스트 케이스로 실행하라**는 의미를 가짐 

  * public 이어야 한다.
  * void를 반환해야 한다.

<br><br>

# NUnit 생명주기

`FixtureLifeCyclce`은 `TestFixture`의 생명주기를 지정할 수 있는 어트리뷰트로 2가지 방식을 설정할 수 있다.

* `LifeCycle.SingleInstance`: 하나의 인스턴스만 생성하여 전체 테스트 케이스에 공유된다. **기본값**
* `LifeCycle.InstancePerTestCase`: 각 테스트 케이스마다 새 인스턴스를 생성한다.

아래의 예시는 SingleInstance로 설정하면 실패하고 InstancePerTestCase로 설정하면 성공하는 테스트를 확인할 수 있다.

```C#
[TestFixture]
[FixtureLifeCycle(LifeCycle.InstancePerTestCase)] // 성공
//[FixtureLifeCycle(LifeCycle.SingleInstance)] // 실패
public class LifeCycleTests
{
    int number = 0;

    [Test]
    public void Test1()
    {
        // 다른 테스트에서 number의 값이 변경되면 실패한다
        Assert.That(number, Is.EqualTo(0));

        number = 1;
    }

    [Test]
    public void Test2()
    {
        // 다른 테스트에서 number의 값이 변경되면 실패한다
        Assert.That(number, Is.EqualTo(0));

        number = 1;
    }
}
```


## 