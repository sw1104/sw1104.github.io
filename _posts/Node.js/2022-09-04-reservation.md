---
title: "2차 프로젝트 숙소 예약기능"
excerpt: "숙소 예약 기능을 알아보쟈"

categories:
  - Node.js
tags:
  - [Node.js]

permalink: /Node.js/2st-project-reservation/

toc: true
toc_sticky: true

date: 2022-09-04
last_modified_at: 2022-09-04
---

# 현재 상태

예약 기능을 생각 했을 때 원했던 많은 기능들은 잘 구현되어 잘 작동되는 되는 상태이며, 변수명이나 코드의 지저분함이 있어서 이 부분은 리팩토링 과정을 거치면서 깔끔한 코드를 만들어볼 생각이다.

```javascript
const getReservationDao = require('../models/reservationDao');
const BaseError = require('../utils/baseError')

const getReservation = async(userId, placeId , availableFrom, availableUntil, guestNumber) => {
        const rentStart = new Date(availableFrom) // 게스트가 입력한 숙박 일정 체크인 날짜
        const rentEnd = new Date(availableUntil) // 게스트가 입력한 숙박 일정 체크아웃 날짜
        const result = (rentEnd.getTime()-rentStart.getTime())/(1000*60*60*24); // 게스트가 며칠 숙박하는지 구하기

        const maxCapacityAndDaysAndPriceAvailableFromAndAvailableUntil = await getReservationDao.getMaxCapacityAndDaysAndPriceAvailableFromAndAvailableUntil(placeId) // dao에서 숙소의 최대 인원, 최대 숙박 가능 인원, 1박 가격, 숙소 예약 가능한 기간 가져오기
        const reservationShcedule = await getReservationDao.getShceduleReservation(placeId) // 숙소에 이미 등록 되어있는 예약 일정 가져오기

        const maxCapacity = Object.values(maxCapacityAndDaysAndPriceAvailableFromAndAvailableUntil[0])[0] // 숙박 최대 인원
        const maxDays = Object.values(maxCapacityAndDaysAndPriceAvailableFromAndAvailableUntil[0])[1] // 숙박 가능한 최대 일 수
        const price = Object.values(maxCapacityAndDaysAndPriceAvailableFromAndAvailableUntil[0])[2] // 1박당 가격
        const rentFrom = Object.values(maxCapacityAndDaysAndPriceAvailableFromAndAvailableUntil[0])[3] // 숙박 가능한 시작일
        const rentTo = Object.values(maxCapacityAndDaysAndPriceAvailableFromAndAvailableUntil[0])[4] // 숙박 가능한 마지막일
        
        const totalPrice = result*price; // 총 가격
        
        let startReservation
        let endReservation
        if(!(reservationShcedule[0] === undefined)){
            startReservation = Object.values(reservationShcedule[0])[0]
            endReservation = Object.values(reservationShcedule[0])[1]
        } // 숙소에 이미 에약된 일정이 있으면 가져올 수 있도록 구현

        if(
            0 === guestNumber|| // 숙박 인원이 0명 이거나
            guestNumber > maxCapacity || // 숙박 인원이 최대 인원보다 많거나
            0 === result || // 숙박 일이 0일 이거나
            result > maxDays || // 숙박 일이 최대 숙박 가능한 일보다 많거나
            rentStart <= rentFrom || // 체크인이 숙박 가능 일보다 앞이거나
            rentEnd >= rentTo || // 체크아웃이 숙박 가능 일보다 뒤이거나
            startReservation <= rentFrom && endReservation >= rentTo || 
            startReservation <= rentTo <= endReservation ||
            startReservation <= rentFrom <= endReservation ||
            startReservation >= rentFrom && endReservation <= rentTo   // 숙박 시작일이 이미 예약된 일정과 겹치는 4가지 상황이면
            ){
                throw new BaseError("VALIDATION_FAILED", 400) // 에러를 던진다.
        }
        // 위의 유효성 검사를 마치면 dao로 넘어가서 정상적으로 예약이 이루어진다.
        return await getReservationDao.getReservation(userId, placeId, availableFrom, availableUntil, guestNumber, totalPrice);
}

module.exports = {
    getReservation
}
```
