---
layout: post
title: "2018년 카카오(KAKAO) 신입공채 1차 코딩테스트 자바스크립트 풀이(1)"
tags: [TIL, javascript, coding]
comments: true
---
스터디 목적으로 문제별 사용되는 알고리즘 유형을 선행 공부하고 풀이합니다.
1년 전 팀과 같이 풀이하고 리뷰했었는데 코드를 따로 저장하지 않아 다시 한 번 풀어 정리했습니다. 코드 리뷰는 언제나 환영합니다!

1차 합격 기준은 문제에 상관없이 **총 7문제 중 4문제 이상**을 맞춰야합니다. 
자세한 정보와 문제 설명은 
[카카오 공식 블로그](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/){:target="_blank"}를 참고해주세요.


## 1. 비밀 지도(난이도: 하)
[비트 연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators){:target="_blank"}를 활용하여 풀이
```javascript
function secretMap(n, arr1, arr2){
    const result = []

    // 비트연산자 비교(a|b)하여 2진수로 변환(toString)후 
    // 원하는 결과 값으로 문자바꾸기(replace)
    for (let i=0; i<n; i++) {
        const binary = (arr1[i] | arr2[i]).toString(2);
        const str = binary.replace(/0/g, " ").replace(/1/g, "#");
        result.push(str);
    }
    return result;
}

//test case
secretMap(5, [9, 20, 28, 18, 11], [30, 1, 21, 17, 28])
secretMap(6, [46, 33, 33 ,22, 31, 50], [27 ,56, 19, 14, 14, 10])
```

## 2. 다트 게임(난이도: 하)
[정규표현식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%EC%A0%95%EA%B7%9C%EC%8B%9D){:target="_blank"}을 활용하여 풀이
```javascript
function dartGame(dartResult){
    let result;
    let currentStep = 0; // 문제 조건상 currentStep < 3입니다
    
    // 점수, 보너스옵션 분리
    const scoreArr = dartResult.match(/\d+/g);
    const bonusArr = dartResult.match(/[SDT*#]/g);

    // 제곱/특별옵션 계산
    for (let i=0; i<bonusArr.length; i++) {
        switch(bonusArr[i]){
            // S,D,T: 제곱옵션
            case "S": scoreArr[currentStep] = (Math.pow(scoreArr[currentStep], 1)); break;
            case "D": scoreArr[currentStep] = (Math.pow(scoreArr[currentStep], 2)); break;
            case "T": scoreArr[currentStep] = (Math.pow(scoreArr[currentStep], 3)); break;

            // *,#: 특별옵션 (*: 현재와 이전점수 *2, #: 현재점수 *-1)
            case "*": for (let ii=0; ii<2; ii++) {scoreArr[currentStep - ii] *= 2}; break;
            case "#": scoreArr[currentStep] *= -1; break;
            default: 
                break;
        }
        
        // 다음이 특별옵션(*,#)이 아닐 경우에만 현재 스텝(currentStep) 업데이트
        if (bonusArr[i+1] !== "*" && bonusArr[i+1] !== "#") currentStep++;
    }
    // scoreArr 합산
    result = scoreArr[0] + scoreArr[1] + scoreArr[2]
    return result;
}

//test case
dartGame("1S2D*3T") //37
dartGame("1D2S#10S") //9
dartGame("1D2S0T") //3
dartGame("1S*2T*3S") //23
dartGame("1D#2S*3S") //5
dartGame("1T2D3D#") //-4
dartGame("1D2S3T*") //59

```

## 3. 캐시(난이도: 하)
[LRU](https://gomguard.tistory.com/115){:target="_blank"} 이론을 처음 접해서 어려워보였지만, 실제로는 나름 간단한 문제네요!
```javascript
function checkCacheTime(cacheSize, cities){
    let resultTime = 0;
    let citieArr = [];
    let cityStr;

    // 파라미터 배열 크기만큼 반복
    // cache miss : 중복 내용의 테이터가 없을때 (+5)
    // cache hit : 중복 내용의 데이터가 있을때 (+1)
    for (let i=0; i<cities.length; i++) {     
        cityStr = cities[i].toLowerCase()

        if(citieArr.length < cacheSize){
            citieArr.push(cityStr);
            resultTime += 5;
        }else {
            if(citieArr.indexOf(cityStr) < 0) {
                citieArr.shift();
                citieArr.push(cityStr);
                resultTime += 5;
            } else {
                resultTime += 1;
                citieArr.splice(citieArr.indexOf(cityStr), 1);
                citieArr.push(cityStr);
            }
        }
    }

    return citieArr, resultTime;
}


//test case
checkCacheTime(3, ["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "Jeju", "Pangyo", "Seoul", "NewYork", "LA"]); //50
checkCacheTime(3, ["Jeju", "Pangyo", "Seoul", "Jeju", "Pangyo", "Seoul", "Jeju", "Pangyo", "Seoul"]); //21
checkCacheTime(2, ["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "SanFrancisco", "Seoul", "Rome", "Paris", "Jeju", "NewYork", "Rome"]); //60
checkCacheTime(5, ["Jeju", "Pangyo", "Seoul", "NewYork", "LA", "SanFrancisco", "Seoul", "Rome", "Paris", "Jeju", "NewYork", "Rome"]); //52
checkCacheTime(2, ["Jeju", "Pangyo", "NewYork", "newyork"]); //16
checkCacheTime(0, ["Jeju", "Pangyo", "Seoul", "NewYork", "LA"]); //25
```