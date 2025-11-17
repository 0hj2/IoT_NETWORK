# 🌦️ 실시간 날씨 모니터링 & LED 제어 시스템

본 프로젝트는 **MQTT**와 **웹 GUI**를 활용하여 실시간으로 날씨 관측 데이터를 가져오고, LED를 통해 상태를 시각화하는 시스템입니다.

---

## 🔧 주요 기능

1. **실시간 기상 데이터 수집**
   - 🌡️ 온도, 💧 습도, 🌧️ 강수량, 🌨️ 강수형태
   - 공공 기상 API 활용 (단기 예보 및 초단기 관측)
   - 5초 주기 자동 업데이트

2. **MQTT 기반 통신**
   - 🔌 Broker와 연결하여 데이터 Publish/Subscribe
   - 토픽 구조 예시:
     - `weather/observation/temp` → 온도
     - `weather/observation/humi` → 습도
     - `weather/observation/rainfall` → 강수량
     - `weather/observation/precipType` → 강수형태
   - `weather/actuator/led` 구독으로 LED 상태 제어

3. **웹 GUI 모니터링**
   - 🖥️ 온도, 습도, 강수량, 강수형태 실시간 표시
   - 🔴 LED ON / 🔵 LED OFF 버튼 제공
   - 📊 상태 변경 시 GUI 업데이트

4. **데이터 처리**
   - Jsoup 라이브러리로 XML 파싱
   - 강수형태(PTY) 코드 변환:
     - 0 → 강수 없음
     - 1 → 비옴
     - 2 → 비/눈
     - 3 → 눈옴
     - 5 → 빗방울
     - 6 → 빗방울눈날림
     - 7 → 눈날림
     - 기타 → 알 수 없음

---

## 📂 파일 구조

- `proj_MqttPublisher_API.java` – MQTT Publisher/Subscriber 및 데이터 처리
- `index.html` – 실시간 날씨 모니터링 웹 GUI
- `js/` – GUI 기능 JS 파일 (socket.io, 실시간 업데이트 등)
- `css/` – GUI 스타일 파일 (Bootstrap, Vegas 등)

---

## ⚙️ 설치 및 실행

### 1. 필요 라이브러리
- [Eclipse Paho MQTT](https://www.eclipse.org/paho/) (Java MQTT 클라이언트) 📡
- [Jsoup](https://jsoup.org/) (HTML/XML 파싱) 📄
- [Node.js + Socket.io](https://socket.io/) (웹 소켓 실시간 통신) 🌐

### 2. MQTT Publisher 실행
java -cp target/classes MqttPublisher_API.proj_MqttPublisher_API

### 3. 웹 서버 실행
node server.js

### 4. 브라우저 접속
http://localhost:3000

---

## 📡 MQTT 토픽 & 명령
| 토픽                             | 설명               |
| ------------------------------ | ---------------- |
| weather/observation/temp       | 🔥 현재 온도 값       |
| weather/observation/humi       | 💧 현재 습도 값       |
| weather/observation/rainfall   | 🌧️ 현재 강수량       |
| weather/observation/precipType | 🌨️ 현재 강수형태      |
| weather/actuator/led           | 💡 LED ON/OFF 제어 |

---
## 🌐 웹 GUI 
- LED ON 버튼 → LED 상태 켜짐
- LED OFF 버튼 → LED 상태 꺼짐
- 실시간 온습도, 강수량, 강수형태 표시
- 강수형태는 PTY 코드 기반으로 사람이 읽을 수 있는 형태로 변환

--- 
## 참고 사항
- 데이터 갱신 주기: 5초
- PTY 코드 정보: 공공 API 기준
- MQTT Broker는 로컬(127.0.0.1)에서 기본 포트 1883 사용
