---
title: Data Structure 자료구조
category: TIL
tag: CS
---

>메모리를 효율적으로 사용하기 위한 데이터 구조

>참고 사이트 : [비쥬얼고](visualgo.net)

# List

## Array List

배열을 이용해 리스트를 만들면 새로 추가할 데이터를 위한 자리를 만들거나 삭제한 데이터가 만든 남은 자리를 메꿔야하기 때문에 데이터를 추가하거나 삭제할 때 많은 시간이 걸린다.
인덱스를 이용해 데이터를 빠르게 검색할 수 있다.
인덱스가 빠짐없이 나열되어 있어 데이터의 크기를 빠르게 알 수 있다.
배열의 한계를 넘는 크기의 인덱스를 추가하면 에러가 발생한다.

메모리상에 각 요소들이 연속적으로 존재한다.

## Linked List

메모리상에 각 요소가 흩어져서 존재하지만 서로 연결이 되어있다. 각 요소는 node 혹은 vertex라고 부른다. node안에는 data field와 link field의 두 변수가 있다. link field엔 다음 node가 무엇인지 저장한다. 첫번째 node를 가리키는 데이터를 갖고 있는 것을 HEAD라고 한다.
array list와 반대로 데이터를 빠르게 추가하거나 삭제할 수 있지만 특정위치에 있는 요소를 찾기 위해서 node를 타고 타고 움직여야하기 때문에 데이터 검색을 하는데 오래 걸린다.