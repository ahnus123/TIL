# 배열(Arrays)

### 배열(Array)

+ 배열(Array)

  ```같은 타입의 데이터를 나열한 선형 자료구조```

  + 특징

    + 연속된 메모리 공간에 순차적으로 저장
    + 배열의 크기는 고정됨. 선언할 때 배열의 크기를 정하고, 변경할 수 없음.
    + 배열이 선언되면 메모리가 Stack 영역에 생성됨 → 그 영역에는 값이 아닌 Heap의 주소 값이 들어감
    + new 연산자를 통해 실제 데이터를 저장하는 메모리가 배열의 크기만큼 할당됨 → 이 메모리들은 Heap 영역에 생성됨
    + 기본으로 초기화되는 값 : 정수(0) / 실수(0.0) / 참조형(null) / 문자('')

  + 용어

    + 요소(element) : 배열을 구성하는 각각의 값
    + 인덱스(index) : 배열에서 위치를 가리키는 숫자

  + 시간 복잡도

    + 삽입/삭제
      + 배열 맨 앞에 삽입/삭제 : `O(n)`
      + 배열 맨 뒤에 삽입/삭제 : `O(1)`
      + 배열 중간에 삽입/삭제 : `O(n)`
    + 탐색 : O(1)

  + 장점

    + 인덱스를 가지고 있어 바로 접근이 가능함
    + 연속된 메모리 공간에 존재하기 때문에 관리하기 용이함

  + 단점

    + 삽입/삭제가 어렵고 오래 걸림 → 원소를 삽입/삭제하는 경우, 해당 원소 이후의 모든 원소들을 이동해야함
    + 배열의 크기 변경이 안됨
    + 연속된 메모리이기 대문에 공간 낭비가 발생함
    + 연속적인 메모리 할당이 필요함

  + 사용하는 경우

    + 데이터 개수가 확실하게 정해져 있을 때
    + 데이터의 삽입/삭제가 적을 때
    + 검색해야할 때

  + 메소드

    | 함수명         | 설명                                                         |
    | -------------- | ------------------------------------------------------------ |
    | binarySearch() | 배열에서 특정 객체의 위치를 이진 검색 알고리즘을 사용해 검색한 후, 해당 위치를 반환<br />* 단, 이진 검색 알고리즘을 사용하기 때문에 매개변수로 전달되는 배열이 정렬되어 있어야 함 |
    | copyOf()       | 전달받은 배열의 특정 길이만큼을 새로운 배열로 복사하여 반환  |
    | copyOfRange()  | 전달받은 배열의 특정 범위에 해당하는 요소만을 새로운 배열로 복사하여 반환 |
    | fill()         | 전달받은 배열의 모든 요소를 특정 값으로 초기화               |
    | sort()         | 전달받은 배열의 모든 요소를 오름차순으로 정렬                |
    | equals()       | 전달받은 두 배열이 같은지를 확인                             |
    | asList()       | 전달받은 배열을 고정 크기의 리스트(List)로 변환하여 반환     |

  + Source Code

    <https://github.com/Sunha03/algorithm-java/blob/master/src/datastructure/array.java>

    ```java
    import java.util.Arrays;
    
    public static void main(String[] args) {
        // 배열 선언
        int[] arr1;
        int arr2[];
    
        // 배열 생성 & 할당®®
        int[] arr3 = new int[5];
    
        // 배열 선언 & 초기화
        int[] num1 = {1, 2, 3, 4, 5};
        int[] num5 = new int[] {1, 2, 3, 4, 5};
    
        // 배열 조회
        for (int i = 0; i < num1.length; i++) {
            System.out.print(num1[i] + " ");
        }System.out.println();
        for (int n : num1) {
            System.out.print(n + " ");
        }System.out.println();
        // (Outputs) 1 2 3 4 5
    
        // binarySearch(배열, 검색값) : 이진 검색 트리를 이용한 검색
        System.out.println(Arrays.binarySearch(num1, 3));
        // (Outputs) 2
    
        // copyOf(원본배열, 복사할 요소의 개수) : 배열 복사
        int[] copyArr = Arrays.copyOf(num1, 3);
        printArr(copyArr);
        // (Outputs) 1 2 3
    
        // copyOfRange(배열, 복사할 시작 인덱스, 복사할 끝 인덱스) : 배열 복사
        int[] copyOfArr = Arrays.copyOfRange(num1, 2, 4);
        printArr(copyOfArr);
        // (Outputs) 3 4
    
        // fill(배열, 초기화할 값) : 배열 초기화
        int[] fillArr = new int[10];
        Arrays.fill(fillArr, 7);
        printArr(fillArr);
        // (Outputs) 7 7 7 7 7 7 7 7 7 7
    
        // sort() : 배열 정렬(오름차순)
        int[] sortArr = {6, 2, 5, 3, 4, 1};
        Arrays.sort(sortArr);
        printArr(sortArr);
        // (Outputs) 1 2 3 4 5 6
    
        // equals() : 배열 정렬(오름차순)
        int[] equalArr1 = {1, 2, 3};
        int[] equalArr2 = {1, 2, 3};
        System.out.println(Arrays.equals(equalArr1, equalArr2));
        // (Outputs) true
    }
    ```



[참고] https://velog.io/@choiiis/자료구조-배열Array과-리스트List
[참고] https://m.blog.naver.com/nuberus/221416148278
[참고] http://www.tcpschool.com/java/java_api_arrays