---
layout: post
title: Javascript 배열
categories: [Javascript]
comments: true
---

배열의 선언과 운용에 대해서는 다른 객체 기반 프로그래밍 언어의 상황과 비슷하거나 같다.
다만, 자바스크립트의 특성인 자료형의 자유성에 의해 배열 요소의 다양성 때문에, 몇가지 고려할 사항이 있다.

-----------------

배열 인덱스 객체의 자료형(배열의 선언)

자바스크립트는 변수의 자료형에 있어서 상당히 자유로운 언어이다. 배열 역시 변수의 집합이므로, 자료형에서 자유롭다고 볼 수 있다.
그 증거로 아래의 예제는 정상적으로 실행되는 예제이다. 

{% highlight javascript %}
// 객체 선언 및 속성 추가
var Users = [
    {pname:'소녀시대', memCnt:9}
    ,{pname:'아이유', age:26}
];
 
//  함수 정의
var add = function(a, b){
    return a+b;
}; // 변수 대입문이기 때문에 함수 정의에도 불구하고 (;) 을 삽입.
 
Users.push(add); // 해당 배열 객체의 가장 뒤에 새로운 속성을 추가.
 
// 
console.log('배열의 요소의 수 : %d', Users.length);
console.log('세 번째 요소로 추가된 함수 실행 : %d', Users[2](10, 10));
{% endhighlight %}

Users 배열 객체의 0, 1, 2 의 인덱스는 전부 서로 다른 객체이다.
Users = (
    0 : [name, memCnt]
    1 : [name, age]
    2 : [(function)]
)

자료형에 민감한 C언어 같은 언어에서는 사용할 수 없는 형식으로 배열의 선언이 가능하다.

-----------------------

배열의 순회

배열의 순회를 하는 방법은 어느 언어를 막론하고, 가장 쉬운 방법인 for문을 예로 들 수 있다.
또한 조금 더 세련된 방법인, forEach문을 사용하는 예도 있다.

{% highlight javascript %}
// 객체 선언 및 속성 추가
var Users = [
    {pname:'소녀시대', memCnt:9}
    ,{pname:'걸스데이', memCnt:4}
    ,{pname:'여자친구', memCnt:6}
];
 
// 배열 인덱스로 순회
console.log('인덱스로 순회');
console.log('배열 요소의 수 : %d', Users.length);
for(var i=0; i<Users.length; i++){
    console.log('배열 요소 #' + i + ' : %s', Users[i].pname);
}
 
// 배열 forEach문으로 순회
console.log('\nforEach 문으로 순회');
Users.forEach(function(item, index){ // item, index 인자의 순서는 바뀌면 안됨.
    console.log('배열 요소 #' + i + ' : %s', Users[i].pname);
});
{% endhighlight %}

-------------------------------------

인덱스의 추가/삭제

|속성 / 메소드 이름             | 설명                          |
|------------------------------|------------------------------|
|push(object)                  |배열의 끝에 요소를 추가한다|
|pop()                          |배열의 끝에 있는 요소를 삭제한다|
|unshift()                      |배열의 앞에 요소를 추가한다|
|shift()                        |배열의 앞에 있는 요소를 삭제한다|
|splice(index, removeCount, [Object])|여러 개의 객체를 요소로 추가하거나 삭제한다|
|slice(index, copyCount)        |여러 개의 요소를 잘라내어 새로운 배열 객체로 만든다|

