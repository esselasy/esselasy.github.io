---
title: "Unity Test Runner"
date: 2019-07-12 20:30:00 +0900
categories: 유니티
tags: 테스트 번역
---
[raywenderlich.com](https://www.raywenderlich.com/)의 [Introduction To Unity Unit Testing](https://www.raywenderlich.com/9454-introduction-to-unity-unit-testing)을 정리하였다.

예제 게임 코드를 다운받으려면 위의 사이트에 가입해야 한다.

## 유닛 테스트(Unit Test)란 무엇인가
유닛 테스트란 코드 내의 작은 기능 하나에 대해 테스트하는 함수이다.

유닛 테스트를 작성할 때는 
- 하나의 테스트는 한 가지 기능만 테스트하도록 작성한다.
- 특정 부분 코드가 특정 시나리오에서 기대한 대로 동작하는 지 검증할 수 있도록 테스트를 설계한다.

## 유닛 테스트 예제
사용자로부터 이름을 입력받는 함수에 대한 유닛 테스트를 만들어보자.
````
public string name = ""
public void UpdateNameWithCharacter(char: character)
{
    // 1
    if (!Char.IsLetter(char))
    {
        return;
    }

    // 2
    if (name.Length > 10)
    {
        return;
    }

    // 3
    name += character;
}
````

어떤 동작이 테스트되어야 하는지 생각해서 유닛 테스트 함수들을 작성한다.
유닛 테스트 함수명은 테스트 내용이 다 포함되도록 길게 짓는 것이 좋다.

- **Unit Test 1**: 10글자 이상이면 name에 추가되지 않는다.
`UpdateNameDoesntAllowCharacterAddingToNameIfNameIsTenOrMoreCharactersInLength()`
- **Unit Test 2**: 입력된 글자가 name에 추가된다.
`UpdateNameAllowsLettersToBeAddedToName()`
- **Unit Test 3**: 글자가 아니면 name에 추가되지 않는다.
`UpdateNameDoesntAllowNonLettersToBeAddedToName()`

![유닛 테스트](https://koenig-media.raywenderlich.com/uploads/2018/12/UnitTesting.png)

유닛 테스트들을 그룹으로 묶어서 Test Suite를 만든다.
유닛 테스트 중 하나라도 실패하면 전체 테스트가 실패한다.

## 예제 게임
![1](https://koenig-media.raywenderlich.com/uploads/2018/12/1.jpg)
![2](https://koenig-media.raywenderlich.com/uploads/2018/12/2.jpg)
![3](https://koenig-media.raywenderlich.com/uploads/2018/12/3.jpg)

## Unity Test Runner 시작하기
유니티에서 **Windows ▸General ▸Test Runner**를 실행한다.

![4](https://koenig-media.raywenderlich.com/uploads/2018/12/4.jpg)

![1](https://koenig-media.raywenderlich.com/uploads/2018/12/1.gif)

## NUnit과 테스트 폴더 설정
유니티에서 제공하는 유닛 테스트 기능은 C#에서 사용되는 [NUnit]([https://nunit.org/](https://nunit.org/)) 유닛 테스트 프레임워크를 사용한다.

유닛 테스트를 위한 Tests 폴더를 만든다.
1. Test Runner Window에서 **PlayMode**를 선택한다.
2. **Projects**의 **Assets**에서 테스트 폴더를 생성하고자 하는 위치를 클릭한다.
3. **CreatePlayMode Test Assembly Folder** 버튼을 클릭하면 **Tests** 폴더가 생성된다.

![플레이모드 테스트 폴더 생성](https://koenig-media.raywenderlich.com/uploads/2018/12/unittest1.gif)

**PlayMode**는 게임 플레이를 테스트할 때 사용되고, **EditMode**는 주로 Custom Inspector를 테스트할 때 사용된다.

## Test Suite
유닛 테스트를 함수로 작성하므로, 이러한 유닛 테스트들을 담을 클래스가 필요하다.
Test Runner는 모든 테스트 클래스에 있는 유닛 테스트들을 실행한다.
테스트하고자 하는 함수별로 묶거나, 기능 단위로 묶은 Test Suite로 테스트 클래스를 만든다.

## Test Assembly와 Test Suite 설정
**Tests** 폴더를 선택하고 **Test Runner** 윈도우에서 **Create Test Script in current folder**를 누른다. 추가된 파일명을 **TestSuite**라고 하자.
![테스트 파일 추가](https://koenig-media.raywenderlich.com/uploads/2018/12/unittest2.gif)

**Tests** 폴더에 보면 **Tests.asmdef**라는 어셈블리 정의 파일이 같이 생성되어 있다. 유니티에서 테스트 파일이나 테스트 코드가 제대로 나오지 않을 경우 테스트 어셈블리 정의 파일이 있는지 확인한다.
테스트 코드가 게임 프로젝트에 있는 코드를 액세스하려면 게임 프로젝트의 어셈블리 정의 파일을 만들어서 테스트 어셈블리에 연결해야 한다.
Scripts 폴더에서 오른쪽 버튼을 클릭한 뒤 **Create▸Assembly Definition**을 실행한다.

![어셈블리 정의 클릭](https://koenig-media.raywenderlich.com/uploads/2018/12/20-1.jpg)

**GameAssembly**라는 이름의 어셈블리 정의 파일을 만든다.

![게임 어셈블리 정의 생성](https://koenig-media.raywenderlich.com/uploads/2018/12/unittest3.png)

**Tests**폴더의 **Tests** 어셈블리 정의 파일을 클릭한다. **Inspector**의 **Assembly Definition References**에서 **+**버튼을 누른다.

![어셈블리 참조 추가](https://koenig-media.raywenderlich.com/uploads/2019/01/select-add-assembly-reference-button.png)

![게임 어셈블리 선택](https://koenig-media.raywenderlich.com/uploads/2019/01/select-game-assembly-asset.jpg)

![추가한 어셈블리 참조 적용](https://koenig-media.raywenderlich.com/uploads/2019/01/apply-reference-change-for-assembly.png)

## 첫번째 Unit Test 작성
운석이 아래로 떨어지는지 테스트하는 코드를 작성해보자. **TestSuite** 파일을 열어서 코드를 다음과 같이 수정한다.
````
using UnityEngine;
using UnityEngine.TestTools;
using NUnit.Framework;
using System.Collections;

public class TestSuite
{
	private Game game;

	// 1
	[UnityTest]
	public IEnumerator AsteroidsMoveDown()
	{
	    // 2
	    GameObject gameGameObject = 
	        MonoBehaviour.Instantiate(Resources.Load<GameObject>("Prefabs/Game"));
	    game = gameGameObject.GetComponent<Game>();
	    // 3
	    GameObject asteroid = game.GetSpawner().SpawnAsteroid();
	    // 4
	    float initialYPos = asteroid.transform.position.y;
	    // 5
	    yield return new WaitForSeconds(0.1f);
	    // 6
	    Assert.Less(asteroid.transform.position.y, initialYPos);
	    // 7
	    Object.Destroy(game.gameObject);
	}
}
````

1. **[UnityTest] Attribute**은 유니티에게 이 함수가 유닛 테스트 코드라는 것을 알려준다. **Test Runner** 윈도우에 이 테스트 함수가 표시된다.
2. `SpawnAsteroid()`로 운석을 하나 만들고 처음 위치를 기록한다.
3. **UnityTest** 함수는 코루틴이므로 **yield return**이 있어야 한다. 운석이 떨어지려면 시간이 흘러야 하므로 0.1초를 기다린다. 기다릴 필요가 없으면 null을 리턴하면 된다.
4. **Assert**를 이용해서 운석의 위치가 처음 위치보다 아래인지 확인한다. NUnit은 다양한 assertion 함수들을 제공한다. 이 assertion으로 테스트 통과 여부가 결정된다.
5. 생성한 게임 오브젝트를 꼭 삭제한다.

## 테스트 실행하기
**Test Runner** 윈도우에 작성한 **AsteroidsMoveDown**이 나타난다.
![10](https://koenig-media.raywenderlich.com/uploads/2018/12/10.jpg)

**Run All** 버튼을 누르면 임시 Scene이 생성되어 테스트가 실행된다.
![11](https://koenig-media.raywenderlich.com/uploads/2018/12/11.jpg)

테스트에 통과하면 녹색 체크로 표시된다.
![12](https://koenig-media.raywenderlich.com/uploads/2018/12/12.jpg)

## 통합 테스트
![통합 테스트](https://koenig-media.raywenderlich.com/uploads/2018/12/IntegrationTests-1.png)

## Test Suite에 다른 테스트 추가
우주선이 운석과 충돌하면 게임이 끝나는지 테스트한다. **TestSuite** 파일에 다음 함수를 추가한다.
````
[UnityTest]
public IEnumerator GameOverOccursOnAsteroidCollision()
{
    GameObject gameGameObject = 
       MonoBehaviour.Instantiate(Resources.Load<GameObject>("Prefabs/Game"));
    Game game = gameGameObject.GetComponent<Game>();
    GameObject asteroid = game.GetSpawner().SpawnAsteroid();
    //1
    asteroid.transform.position = game.GetShip().transform.position;
    //2
    yield return new WaitForSeconds(0.1f);

    //3
    Assert.True(game.isGameOver);

    Object.Destroy(game.gameObject);
}
````
1. 생성된 운석의 위치를 우주선의 위치와 같게 한다.
2. 물리 엔진이 충돌을 판단하는데 시간이 걸리므로 0.1초를 기다린다.
3. `gameOver`가 `true`인지 확인한다.

**Test Runner** 윈도우에 새로 추가한 함수가 유닛 테스트 목록에 나타난다.
![13](https://koenig-media.raywenderlich.com/uploads/2018/12/13.jpg)

**GameOverOccursOnAsteroidCollision** 함수를 클릭하고 **Run Selected**를 누르면 선택된 함수만 실행된다.
![14](https://koenig-media.raywenderlich.com/uploads/2018/12/14.jpg)

## Setup과 TearDown 추가
두 테스트 함수를 보면 game 게임 오브젝트를 생성하는 부분과 삭제하는 부분이 중복으로 들어가 있다.
````
GameObject gameGameObject = MonoBehaviour.Instantiate(Resources.Load<GameObject>("Prefabs/Game"));
game = gameGameObject.GetComponent<Game>();
...
Object.Destroy(game.gameObject);
````

대부분의 유닛 테스트에는 이와 같이 **Setup**과 **Tear Down** 단계가 있는 경우가 많다.
**[Setup] Attribute**은 매 유닛 테스트 함수를 실행하기 전에 준비작업을 위해, **[TearDown] Attribute**은 실행한 뒤 정리작업을 위해 사용한다.

````
public class TestSuite
{
    private Game game;

    [SetUp]
    public void Setup()
    {
        GameObject gameGameObject = 
            MonoBehaviour.Instantiate(Resources.Load<GameObject>("Prefabs/Game"));
        game = gameGameObject.GetComponent<Game>();
    }

    [TearDown]
    public void Teardown()
    {
        Object.Destroy(game.gameObject);
    }

    [UnityTest]
    public IEnumerator AsteroidsMoveDown()
    {
        GameObject asteroid = game.GetSpawner().SpawnAsteroid();
        float initialYPos = asteroid.transform.position.y;
        yield return new WaitForSeconds(0.1f);
  
        Assert.Less(asteroid.transform.position.y, initialYPos);
    }

    [UnityTest]
    public IEnumerator GameOverOccursOnAsteroidCollision()
    {
        GameObject asteroid = game.GetSpawner().SpawnAsteroid();
        asteroid.transform.position = game.GetShip().transform.position;
        yield return new WaitForSeconds(0.1f);

        Assert.True(game.isGameOver);
    }
}
````

Setup에서 Scene을 불러와서 사용할 수도 있다.
````
    [SetUp]
    public void Setup()
    {
        SceneManager.LoadScene("GameScene");
    }
````
## Test Driven Development (TDD)
테스트 코드를 먼저 작성한 다음에 프로그램을 작성하는 개발 기법이다.

## Unit Test의 장점
- 함수가 기대한 대로 동작할 거라는 확신을 준다.
- 테스트 가능한 코드를 작성하게 된다.
- 버그를 더 빠르게 찾아내고 고칠 수 있다.
- 기존에 동작하던 코드에 업데이트로 인한 새로운 버그가 생기는 것을 방지할 수 있다. (**regression bugs**)

## Unit Test의 단점
- 프로그램 코드를 작성하는 것보다 테스트 코드를 작성하는 게 더 오래걸릴 수도 있다.
- 틀린 테스트 코드로 인해 잘못된 확신이 들 수 있다.
- 테스트 코드를 작성하려면 추가적인 지식이 필요하다.
- 코드의 중요한 부분들은 테스트하기 쉽지 않다.
- private 클래스 함수들을 테스트할 수 없는 테스트 프레임워크도 있다.
- 테스트 코드 유지보수에 시간이 많이 걸린다.
- 통합 모듈 에러 (**integration error**)를 찾아내기 어렵다.
- UI는 테스트하기 힘들다.
- 경험이 부족한 개발자는 이상한 걸 테스트하기 위해 시간을 낭비할 수 있다.
- 외부 모듈이나 런타임 의존성이 있을 경우 테스트하기 어렵다.

## 마치며

이 튜토리얼이 도움이 되었다면 [raywenderlich.com](https://www.raywenderlich.com/)에 가입하고 이 글 [Introduction To Unity Unit Testing](https://www.raywenderlich.com/9454-introduction-to-unity-unit-testing)에 별점을 매겨주세요.

> Written with [StackEdit](https://stackedit.io/).
