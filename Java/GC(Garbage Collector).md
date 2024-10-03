# [Java] GC(Garbage Collector)

### GC(Garbage Collector)란?

Garbage Collector(GC)는 JVM 메모리 관리 시스템의 일부로, 더 이상 참조되지 않는 객체를 메모리에서 자동으로 제거하여 메모리 누수를 방지하고 프로그램의 효율성을 유지하는 역할을 합니다. 이는 C/C++과 같은 언어에서 개발자가 직접 메모리 할당 및 해제를 관리해야 하는 부담을 줄여주며, 개발자가 메모리 관리로 인해 발생할 수 있는 오류를 방지할 수 있습니다.

<br/>

### GC의 장단점

- **장점**
    - **자동 메모리 관리**: 개발자가 직접 메모리를 해제할 필요 없이 자동으로 메모리를 관리해주기 때문에 개발 속도와 생산성을 높일 수 있습니다.
    - **메모리 누수 방지**: 사용하지 않는 객체를 자동으로 제거하여 메모리 누수를 방지하고 시스템 안정성을 높입니다.
- **단점**
    - **Stop the World(STW)**: GC가 실행될 때 모든 애플리케이션 스레드가 일시 정지되며, 이로 인해 성능 저하가 발생할 수 있습니다.
    - **비예측성**: GC는 특정 시점에 예기치 않게 실행되므로, 실시간 응답성이 중요한 애플리케이션에서는 비효율적일 수 있습니다.

<br/>

### GC의 단점을 개선하기 위한 방법

- **GC 튜닝**: 애플리케이션의 특성에 맞는 GC 알고리즘을 선택하고, 각 알고리즘의 파라미터를 조정하여 GC의 성능을 최적화합니다.
- **Heap 메모리 구조 조정**: Heap 크기 및 세대별 메모리 비율 조정 등을 통해 GC의 빈도와 실행 시간을 제어합니다.
- **메모리 할당 패턴 최적화**: 객체 생성 및 참조 패턴을 분석하여 불필요한 객체 생성을 줄이고, 메모리 사용 패턴을 개선하여 GC 성능을 높일 수 있습니다.

<br/>

### JVM의 Heap 메모리 구조

![Untitled](https://github.com/user-attachments/assets/3e20ea61-b31f-4b15-9bac-29a7a7855141)

JVM의 Heap 영역은 **Young Generation, Old Generation, Metaspace**로 나뉩니다.

1. **Young Generation (Young Gen)**
    - 새로 생성된 객체가 할당되는 영역입니다.
    - Young Gen 내에서는 다시 **Eden**과 **Survivor (S0, S1)** 영역으로 나뉩니다.
    - 대부분의 객체는 Young Generation에서 생성되며, 짧은 생명 주기를 갖습니다.
    - Young 영역에서 발생하는 GC를 **Minor GC**라고 합니다.
2. **Old Generation (Old Gen)**
    - Young Generation에서 살아남아 일정 임계값을 넘긴 객체들이 이동하는 영역입니다.
    - Old Generation은 더 큰 메모리 크기를 가지며, 이 영역의 객체들은 비교적 긴 생명 주기를 갖습니다.
    - Old 영역에서 발생하는 GC를 **Major GC** 또는 **Full GC**라고 부릅니다.
3. **Metaspace**
    - 클래스 메타데이터(class metadata)가 저장되는 영역으로, Java 8 이전의 **Permanent Generation**이 Java 8 이후 **Metaspace**로 변경되었습니다.
    - Metaspace는 JVM Heap 외부의 네이티브 메모리에 할당되며, JVM의 다른 Heap 메모리와는 다르게 관리됩니다.

<br/>

### Young 영역에서 Old 영역으로 객체가 이동하는 과정 (Promotion)

1. **객체 생성**: 새롭게 생성된 객체는 Young Generation의 **Eden 영역**에 위치합니다.
2. **Minor GC 발생**: Eden 영역이 가득 차면 Minor GC가 발생하여, Eden 영역의 살아남은 객체는 **Survivor 영역(S0)**으로 이동합니다.
3. **Survivor 영역 교환**: 다음 Minor GC가 발생할 때 Survivor(S0)의 객체는 S1으로 이동하고, 반대로 S1의 객체는 S0으로 이동합니다.
4. **Old Gen으로 Promotion**: Survivor 영역에서 살아남은 객체의 age(생존 연속 횟수)가 특정 임계값(예: 15)에 도달하면, 해당 객체는 **Old Generation**으로 이동합니다. 이 과정을 **Promotion**이라고 부릅니다.

<br/>

## Minor GC

![https://blog.kakaocdn.net/dn/cz9lzz/btrIRGyAeEV/6PTQpYAVBKyAgl29pVK0kk/img.png](https://blog.kakaocdn.net/dn/cz9lzz/btrIRGyAeEV/6PTQpYAVBKyAgl29pVK0kk/img.png)

Young Generation 영역은 짧게 살아남는 메모리들이 존재하는 공간이다.

모든 객체는 처음에는 Young Generation에 생성되게 된다.

Young Generation의 공간은 Old Generation에 비해 상대적으로 작기 때문에 메모리 상의 객체를 찾아 제거하는데 적은 시간이 걸린다. (작은 공간에서 데이터를 찾으니까)

이 때문에 Young Generation 영역에서 발생되는 GC를 **Minor GC**라 불린다.

**1.** 처음 생성된 객체는 Young Generation 영역의 일부인 Eden 영역에 위치

![https://blog.kakaocdn.net/dn/cXg3ZZ/btrITpQET0n/aJSAYDEziQIKlVCvxaSpg0/img.png](https://blog.kakaocdn.net/dn/cXg3ZZ/btrITpQET0n/aJSAYDEziQIKlVCvxaSpg0/img.png)

![https://blog.kakaocdn.net/dn/dnDTuC/btrISOQKFwn/ullhkIEtDiPY7HUahMKvW1/img.png](https://blog.kakaocdn.net/dn/dnDTuC/btrISOQKFwn/ullhkIEtDiPY7HUahMKvW1/img.png)

**2.** 객체가 계속 생성되어 Eden 영역이 꽉차게 되고 Minor GC가 실행

![https://blog.kakaocdn.net/dn/tySvx/btrITpwlB0D/JtcEmTbIYaPnHAAoNU8vB0/img.png](https://blog.kakaocdn.net/dn/tySvx/btrITpwlB0D/JtcEmTbIYaPnHAAoNU8vB0/img.png)

**3.** Mark 동작을 통해 reachable 객체를 탐색

![https://blog.kakaocdn.net/dn/cJl67l/btrIXRd6Izv/ZwTbTqbKkuaH71Llqzog6k/img.png](https://blog.kakaocdn.net/dn/cJl67l/btrIXRd6Izv/ZwTbTqbKkuaH71Llqzog6k/img.png)

**4.** Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동

![https://blog.kakaocdn.net/dn/bxY6Md/btrIWNwtetB/4EpRJFGM5v7Fwk9dk0Wc9K/img.png](https://blog.kakaocdn.net/dn/bxY6Md/btrIWNwtetB/4EpRJFGM5v7Fwk9dk0Wc9K/img.png)

**5.** Eden 영역에서 사용되지 않는 객체(unreachable)의 메모리를 해제(sweep)

![https://blog.kakaocdn.net/dn/ssSMf/btrIWMYFQlZ/KqHBu6rr18ANKo1ngOnMQk/img.png](https://blog.kakaocdn.net/dn/ssSMf/btrIWMYFQlZ/KqHBu6rr18ANKo1ngOnMQk/img.png)

**6.** 살아남은 모든 객체들은 age값이 1씩 증가

![https://blog.kakaocdn.net/dn/Dno6M/btrIPYfc2VC/BLLkKAXLVRYKUAu3R6Vefk/img.png](https://blog.kakaocdn.net/dn/Dno6M/btrIPYfc2VC/BLLkKAXLVRYKUAu3R6Vefk/img.png)

> **[ age 값이란? ]**
> 
> 
> Survivor 영역에서 객체의 객체가 살아남은 횟수를 의미하는 값이며, Object Header에 기록된다.
> 
> 만일 age 값이 임계값에 다다르면 Promotion(Old 영역으로 이동) 여부를 결정한다.
> 
> JVM 중 가장 일반적인 HotSpot JVM의 경우 이 age의 기본 임계값은 31이다.
> 
> 객체 헤더에 age를 기록하는 부분이 6 bit로 되어 있기 때문이다.
> 
> 또한 Survivor 영역의 제한 조건으로 Survivor 영역 중 반드시 1개는 사용되어야 하고, 나머지는 비어 있어야 한다.
> 
> 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 모두 사용량이 0이라면 현재 시스템이 정상적인 상황이 아니라는 반증이 된다.
> 

**7.** 또다시 Eden 영역에 신규 객체들로 가득 차게 되면 다시한번 minor GC 발생하고 mark 한다

![https://blog.kakaocdn.net/dn/cNoD2j/btrIT8gQrk9/vIqyTQJvZ16ByIGplen2D0/img.png](https://blog.kakaocdn.net/dn/cNoD2j/btrIT8gQrk9/vIqyTQJvZ16ByIGplen2D0/img.png)

![https://blog.kakaocdn.net/dn/8XX01/btrIPYsHxbi/FothJXlh95Tc6DSYSkZb10/img.png](https://blog.kakaocdn.net/dn/8XX01/btrIPYsHxbi/FothJXlh95Tc6DSYSkZb10/img.png)

![https://blog.kakaocdn.net/dn/cRhJ1g/btrIXPtPw5o/gN8t1V7BjNXaRqPjKueVX1/img.png](https://blog.kakaocdn.net/dn/cRhJ1g/btrIXPtPw5o/gN8t1V7BjNXaRqPjKueVX1/img.png)

**8.** marking 한 객체들을 비어있는 Survival 1으로 이동하고 sweep

![https://blog.kakaocdn.net/dn/dsj6Og/btrIRHqHljJ/00JVcQ3KD1E8he6qejuLik/img.png](https://blog.kakaocdn.net/dn/dsj6Og/btrIRHqHljJ/00JVcQ3KD1E8he6qejuLik/img.png)

![https://blog.kakaocdn.net/dn/blilhe/btrISO4eRF3/sMVxubaUB19Sm1XsVJE90k/img.png](https://blog.kakaocdn.net/dn/blilhe/btrISO4eRF3/sMVxubaUB19Sm1XsVJE90k/img.png)

**10.** 다시 살아남은 모든 객체들은 age가 1씩 증가

![https://blog.kakaocdn.net/dn/d8v4Vk/btrIXwVy4Uj/iGv5H94KNcZhQ0GufntWg0/img.png](https://blog.kakaocdn.net/dn/d8v4Vk/btrIXwVy4Uj/iGv5H94KNcZhQ0GufntWg0/img.png)

**11.** 이러한 과정을 반복

![https://blog.kakaocdn.net/dn/2WHEq/btrIUaeDqLG/qN0f490z0nGddd0Vl7ECdk/img.gif](https://blog.kakaocdn.net/dn/2WHEq/btrIUaeDqLG/qN0f490z0nGddd0Vl7ECdk/img.gif)

<br/>

## Major GC

![https://blog.kakaocdn.net/dn/Al6rO/btrISO4bXw2/mkrLKFEfQKSWgZKxMcxKWK/img.png](https://blog.kakaocdn.net/dn/Al6rO/btrISO4bXw2/mkrLKFEfQKSWgZKxMcxKWK/img.png)

Old Generation은 길게 살아남는 메모리들이 존재하는 공간이다.

Old Generation의 객체들은 거슬러 올라가면 처음에는 Young Generation에 의해 시작되었으나, GC 과정 중에 제거되지 않은 경우 age 임계값이 차게되어 이동된 녀석들이다.

그리고 **Major GC**는 객체들이 계속 Promotion되어 Old 영역의 메모리가 부족해지면 발생하게 된다.

**1.** 객체의 age가 임계값(여기선 8로 설정)에 도달하게 되면,

![https://blog.kakaocdn.net/dn/c3ZKIh/btrITnE39P2/cL0xncLtqvQxqOmxmyt8V0/img.png](https://blog.kakaocdn.net/dn/c3ZKIh/btrITnE39P2/cL0xncLtqvQxqOmxmyt8V0/img.png)

**2.** 이 객체들은 Old Generation 으로 이동된다. 이를 promotion 이라 부른다.

![https://blog.kakaocdn.net/dn/GJGPb/btrIUmFRmQO/SZDmkA2vbBcqKcXrNiIhOk/img.png](https://blog.kakaocdn.net/dn/GJGPb/btrIUmFRmQO/SZDmkA2vbBcqKcXrNiIhOk/img.png)

**3.** 위의 과정이 반복되어 Old Generation 영역의 공간(메모리)가 부족하게 되면 Major GC가 발생되게 된다.

![https://blog.kakaocdn.net/dn/mnBRi/btrIUm6WXIl/obIsxDpns6wEkkKfjdvLCK/img.gif](https://blog.kakaocdn.net/dn/mnBRi/btrIUm6WXIl/obIsxDpns6wEkkKfjdvLCK/img.gif)

<br/>

### Minor GC와 Major GC의 차이점

![https://blog.kakaocdn.net/dn/bQinW1/btrIOSeHYMd/rd1iSnwNConXoRVWiDvDS0/img.png](https://blog.kakaocdn.net/dn/bQinW1/btrIOSeHYMd/rd1iSnwNConXoRVWiDvDS0/img.png)

- **Minor GC**
    - Young Generation에서 발생하며, 보통 짧은 시간 내에 완료됩니다.
    - 애플리케이션에 미치는 영향이 적고, Old Generation의 객체는 영향받지 않습니다.
- **Major GC / Full GC**
    - Old Generation과 Young Generation 전체를 대상으로 발생합니다.
    - Major GC는 Minor GC보다 시간이 오래 걸리며, 애플리케이션 성능에 큰 영향을 줄 수 있습니다.
    - Major GC는 메모리가 부족하거나 시스템이 불안정할 때 발생합니다.

<br/>

### GC 알고리즘의 종류

- **Serial GC**
    - 단일 스레드를 이용하여 GC를 수행합니다. 모든 스레드를 중지하고 GC를 실행하므로 멀티코어 환경에서는 비효율적입니다.
- **Parallel GC**
    - 여러 개의 스레드를 이용하여 GC를 병렬로 수행합니다. Throughput을 중시하는 애플리케이션에 적합합니다.
- **G1(Garbage First) GC**
    - JVM의 기본 GC로, 힙을 여러 영역으로 나누고, 각 영역에서 가장 많은 가비지를 포함한 영역을 우선적으로 GC합니다. GC의 일관된 응답 시간을 제공하며, 대규모 애플리케이션에 적합합니다.
- **CMS(Concurrent Mark-Sweep) GC**
    - Old Generation에 대해 비동기적으로 수행되는 GC입니다. 애플리케이션의 응답 시간을 중요시하는 경우 적합하지만, 메모리 파편화가 발생할 수 있습니다.
- **ZGC**
    - 초저지연(ultra-low latency) GC로, 매우 짧은 STW 시간을 보장합니다. 대규모 메모리에서 성능을 발휘하며, Java 11 이상에서 사용할 수 있습니다.

<br/>

### `finalize()` 메서드의 문제점

`finalize()` 메서드는 객체가 GC에 의해 제거되기 전에 호출되지만, 다음과 같은 문제를 발생시킬 수 있습니다:

- GC 실행 시점을 제어하지 못하므로 finalize 메서드가 언제 호출될지 예측할 수 없습니다.
- 메모리 해제를 지연시키거나, 자원 해제를 제대로 수행하지 못하여 메모리 누수나 성능 저하를 초래할 수 있습니다.
- 성능상 부담이 크고, 예기치 않은 오류를 야기할 수 있으므로 Java 9 이상에서는 `finalize()` 사용을 비추천하고, 대신 `try-with-resources`나 `Cleaner` 클래스를 사용하여 자원 해제를 관리하는 것이 권장됩니다.

<br/>

### GC 대상이 되는 객체의 조건

변수의 값이 `null`로 설정되었다고 해서 자동으로 GC의 대상이 되는 것은 아닙니다. GC는 **더 이상 참조되지 않는 객체**를 대상으로 하기 때문에, `null`로 설정된 객체가 다른 변수나 데이터 구조에서 참조되고 있다면 GC의 대상이 되지 않습니다.

따라서 GC는 변수의 값 자체가 아닌 객체의 참조 여부를 기준으로 대상을 결정합니다. 객체가 어느 곳에서도 참조되지 않거나, 접근할 수 없는 경우에만 GC의 대상이 됩니다.

<br/>

### GC 튜닝의 예시

1. **Heap 메모리 크기 조정**:
    - `Xms`와 `Xmx` 옵션을 사용하여 초기 Heap 크기와 최대 Heap 크기를 조정합니다.
2. **GC 알고리즘 선택**:
    - `XX:+UseG1GC`, `XX:+UseParallelGC`와 같은 옵션을 사용하여 애플리케이션에 맞는 GC 알고리즘을 설정합니다.
3. **Young Generation 비율 조정**:
    - `XX:NewRatio` 옵션을 통해 Young Generation과 Old Generation의 비율을 조정하여 GC 빈도와 시간을 최적화합니다.
4. **객체 생존 연령 조정**:
    - `XX:MaxTenuringThreshold` 옵션을 사용하여 객체가 Old Generation으로 이동하는 시점을 조정하여 메모리 할당 패턴에 맞는 GC 동작을 설정합니다.

이와 같은 튜닝을 통해 GC의 성능을 개선할 수 있으며, 애플리케이션의 메모리 관리 및 성능 최적화에 큰 도움이 될 수 있습니다.


<br/>
