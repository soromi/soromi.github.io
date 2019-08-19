---
layout: post
title: "2017년 카카오(KAKAO) 신입공채 1차 코딩테스트 자바스크립트 풀이(1)"
tags: [TIL, javascript, coding]
comments: true
permalink: /blog/:title/
---

스터디 목적으로 문제별 사용되는 알고리즘 유형을 선행 공부하고 풀이합니다.
1년 전 팀과 같이 풀이하고 리뷰했었는데 코드를 따로 저장하지 않아 다시 한 번 풀어 정리했습니다.

주로 순서 구상(주석에 내용 정리) 후 순서대로 구현합니다.
코드 리뷰는 언제나 환영합니다!

1차 합격 기준은 문제에 상관없이 **총 7문제 중 4문제 이상**을 맞춰야합니다.
자세한 정보와 문제 설명은
[카카오 공식 블로그](http://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/){:target="\_blank"}를 참고해주세요.

## 1. 비밀 지도(난이도: 하)

[비트 연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators){:target="\_blank"}를 활용하여 풀이

```javascript
function secretMap(n, arr1, arr2) {
	const result = [];
	let binary, str;

	// 비트연산자 비교(a|b)하여 2진수로 변환(toString), 자릿수 맞추기(padStart)
	// 원하는 결과 값으로 문자바꾸기(replace)
	for (let i = 0; i < n; i++) {
		binary = (arr1[i] | arr2[i]).toString(2).padStart(n, "0");
		str = binary.replace(/0/g, " ").replace(/1/g, "#");
		result.push(str);
	}
	return result;
}

//test case
secretMap(5, [9, 20, 28, 18, 11], [30, 1, 21, 17, 28]);
secretMap(6, [46, 33, 33, 22, 31, 50], [27, 56, 19, 14, 14, 10]);
```

## 2. 다트 게임(난이도: 하)

[정규표현식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%EC%A0%95%EA%B7%9C%EC%8B%9D){:target="\_blank"}을 활용하여 풀이

```javascript
function dartGame(dartResult){
    const result;
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

[LRU](https://gomguard.tistory.com/115){:target="\_blank"} 이론을 처음 접해서 어려워보였지만, 실제로는 나름 간단한 문제네요!

```javascript
function checkCacheTime(cacheSize, cities) {
	const resultTime = 0;
	let citieArr = [];
	let cityStr;

	// 파라미터 배열 크기만큼 반복
	// cache miss : 중복 내용의 테이터가 없을때 (+5)
	// cache hit : 중복 내용의 데이터가 있을때 (+1)
	for (let i = 0; i < cities.length; i++) {
		cityStr = cities[i].toLowerCase();

		if (citieArr.length < cacheSize) {
			citieArr.push(cityStr);
			resultTime += 5;
		} else {
			if (citieArr.indexOf(cityStr) < 0) {
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
checkCacheTime(3, [
	"Jeju",
	"Pangyo",
	"Seoul",
	"NewYork",
	"LA",
	"Jeju",
	"Pangyo",
	"Seoul",
	"NewYork",
	"LA"
]); //50
checkCacheTime(3, [
	"Jeju",
	"Pangyo",
	"Seoul",
	"Jeju",
	"Pangyo",
	"Seoul",
	"Jeju",
	"Pangyo",
	"Seoul"
]); //21
checkCacheTime(2, [
	"Jeju",
	"Pangyo",
	"Seoul",
	"NewYork",
	"LA",
	"SanFrancisco",
	"Seoul",
	"Rome",
	"Paris",
	"Jeju",
	"NewYork",
	"Rome"
]); //60
checkCacheTime(5, [
	"Jeju",
	"Pangyo",
	"Seoul",
	"NewYork",
	"LA",
	"SanFrancisco",
	"Seoul",
	"Rome",
	"Paris",
	"Jeju",
	"NewYork",
	"Rome"
]); //52
checkCacheTime(2, ["Jeju", "Pangyo", "NewYork", "newyork"]); //16
checkCacheTime(0, ["Jeju", "Pangyo", "Seoul", "NewYork", "LA"]); //25
```

## 4. 셔틀버스(난이도: 중)

문제 이해를 잘못하고.... 몇 번에 걸쳐 다시 풀었던 문제네요.  
개인적으로 다시 봐도 헷갈리는 문제같습니다. (ㅠㅠ)

```javascript
function shuttleBus(n, t, m, timetable){
    const result;
    let time = getTime("09:00"); // 첫 셔틀버스 시간은 9시이므로
    const cleanTimetable = [];

    function getTime(time){
        return (Number(time[0] + time[1]) * 60) + (Number(time[3] + time[4]));
    }

    // 줄 선 크루 배열 재구성 (String -> Number형으로 변환)
    for(let i = 0; i < timetable.length; i++) {
        cleanTimetable.push(getTime(timetable[i]));
    }

    // 줄 선 크루 배열 오름차순 재배치(sort) 및 마지막 시간
    cleanTimetable.sort((a, b) => a - b);

    // 줄 선 크루 차례대로 시간별(n) 카카오 셔틀버스 타기
    for (let i = 0; i < n; i++) {
        // 탈 수 있는 크루 몇 명인지 필터링
        let goCrews = cleanTimetable.filter(crew => time >= crew).length;

        // 크루들을 태운다
        if (i === n - 1) {
            // 만약, 마지막 운행이고
            // 나머지 인원이 차에 꽉 차서 못탈 경우
            // 그 전에 타야하므로 와야하는 맞춰서 시간 업데이트
            if (goCrews >= m) time = cleanTimetable[m-1] - 1;
        } else {
            // 마지막 운행이 아니라면
            // 탈 수 있는 크루(최대 m명)들 배열에서 삭제(splice)하고, 현재 셔틀버스 시간 업데이트
            cleanTimetable.splice(0, goCrews > m ? m : goCrews);
            time += t;
        }
    }

    // 문제 출력값에 맞는 Number -> String형으로 변환
    result = String(Math.floor(time/60)).padStart(2, "0") + ":" + String(time%60).padStart(2, "0");
    return result;
}

//test case
shuttleBus(1, 1, 5, ["08:00", "08:01", "08:02", "08:03"]) //"09:00"
shuttleBus(2, 10, 2, ["09:10", "09:09", "08:00"]) //"09:09"
shuttleBus(2, 1, 2, ["09:00", "09:00", "09:00", "09:00"]) //"08:59"
shuttleBus(1, 1, 5, ["00:01", "00:01", "00:01", "00:01", "00:01"]) //"00:00"
shuttleBus(1, 1, 1, ["23:59"]) //"09:00"
shuttleBus(10, 60, 45, ["23:59","23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59", "23:59"]) //"18:00"
```

## 5. 뉴스 클러스터링(난이도: 중)

[Jaccard similarity](https://mun-su.github.io/2017/09/28/Algorithm/basic/jaccard_similarity/){:target="\_blank"},
[Clustering](https://ratsgo.github.io/machine%20learning/2017/04/16/clustering/){:target="\_blank"},  
[Array intersection difference and union in ES6](https://medium.com/@alvaro.saburido/set-theory-for-arrays-in-es6-eb2f20a61848){:target="\_blank"}
을 활용하여 풀이

```javascript
function newsClustering(str1, str2) {
	let str1Arr = setArr(str1);
	let str2Arr = setArr(str2);
	let union = 0;
	let intersection = 0;

	// 조건에 맞는 배열 만들기
	function setArr(str) {
		let strArr = [];
		for (let i = 0; i < str.length - 1; i++) {
			// 대문자로 변환후 2개씩 끊어 조각내기
			strPiece = str.toUpperCase().substr(i, 2);

			// 대문자 이외의 문자가 없으면 배열에 추가
			if (!/[^A-Z]/gi.test(strPiece)) strArr.push(strPiece);
		}
		return strArr;
	}

	// 공집합 배열 생성
	let unionArr = [...new Set([...str1Arr, ...str2Arr])];

	// 공집합 내의 원소값들이 각각 몇개가 있는지 체크 (다중집합)
	unionArr.forEach(i => {
		const filterArr1 = str1Arr.filter(x => x === i).length;
		const filterArr2 = str2Arr.filter(x => x === i).length;

		// 공집합(최대값), 교집합(최소값) 체크
		union += Math.max(filterArr1, filterArr2);
		intersection += Math.min(filterArr1, filterArr2);
	});

	// 문제 조건 계산법
	return union === 0 ? 65536 : Math.floor((intersection / union) * 65536);
}

//test case
newsClustering("FRANCE", "french"); //16384
newsClustering("handshake", "shake hands"); //65536
newsClustering("aa1+aa2", "AAAA12"); //43690
newsClustering("1S*2T*3S", "e=m*c^2"); //65536
```

---

###### 코멘트

정확히 이해해야 예외 상황을 마주하지 않겠지만,
긴 문제를 이해하는 것에 집중하다보니 풀이 시간이 조금 지체된 것 같습니다.
비전공자로 부족했던 이론을 공부할 수 있었던 시간이었습니다. 부지런하게 공부해야겠습니다.

나중에 봐도 이해하기 쉽게 하려고 변수명을 구구절절식으로 만들게 되는 듯 헤서 아쉽습니다.
또 ES6을 잘 활용하지 못한 것 같아서 언젠가 수정하는 걸로..
시간이 지나 다시 보면 부끄럽기도 하겠지만 제가 성장했음을 느낄 수 있겠죠? 🙂
