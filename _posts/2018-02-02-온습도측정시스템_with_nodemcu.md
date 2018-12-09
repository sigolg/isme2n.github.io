---
layout: post
title: 온습도측정시스템 구축하기 with nodemcu v2
categories: work
tags: work
---
# 1. 준비하기

## 1) 배경

## 2) IoT 기기 선정

## 3) Firmware 설치

## 4) ESplorer 설치

## 5) 코드 작성

## 6) Thingspeak 연동하기

- pushbullet

## 7) 관련 사이트

- http://www.arduined.eu/ : CH340G 드라이버 모듈
- https://github.com/nodemcu/nodemcu-firmware/releases : nodemcu firmware 사이트
- https://nodemcu.readthedocs.io/en/master/ : nodemcu 각종 명령어
- https://nodemcu.readthedocs.io/en/master/en/build/ : firmware 소스로 binary 만드는 법
- https://odd-one-out.serek.eu/projects/esp8266-nodemcu-dht22-mqtt-deep-sleep/ : MTQQ, DSLEEP 예제
- https://blogs.mathworks.com/iot/2017/01/20/use-mqtt-to-send-iot-data-to-thingspeak/ : MQTT로 thingspeak 메시지 보내기
- https://hackaday.com/2017/10/31/review-iot-data-logging-services-with-mqtt/ : Nodemcu에서 MQTT를 이용하여 thingspeak 메시지 보내기
- http://community.thingspeak.com/forum/thingspeak-api/do-multiple-devices-require-multiple-channels/ : thingspeak 단일채널 접속 제한 관련 글
- http://networkstarts.tistory.com/32: pushbullet으로 알람 보내기
- https://thingspeak.com/prices/thingspeak_home: thingspeak유료 계정 설명

# 2. 구축 일지

## 2018.02.02

### nodemcu v2의 wifi.suspend(), node.sleep() 기능 사용하기

nodemcu v2 + DHT22 + AA건전지4개 조합으로 테스트를 진행했고 잘 동작하였음. 문제는 이틀만에 건전지 방전으로 IoT 단말기가 꺼짐. 상전을 이용하지 않는 상태에서 실시간 온습도를 측정하는 것이 장점인데... 그래서 nodemcu의 sleep 및 suspend 기능을 통해서 해야한다. 그런데 여기서 두번째 문제! 기존에 다운받은 nodemcu firmware에는 두개 기능이 off 되어있다. 사용을 하기위해서는 config 수정 한 firmware를 업로드 해야한다. 누군가 만들어놓은게 있으면 좋은데 찾기가 어렵다.. 그게 안되면 직접 build를 해아한다.
- https://nodemcu.readthedocs.io/en/master/en/build/

위 사이트에 build를 하는법이 있는데, 첫번째 cloud에서 제공하는 build방법은 소스 수정이 불가한 듯 보인다. 그냥 최신 master 버젼을 build하고 그 bin 파일을 주는듯 하다.
### 기존 firmware에서 deep sleep 기능 사용하기

위 방법으로 잘 안되어서 열심히 구글링 해본결과 기존 firmware에서도 deep sleep을 사용 할 수 있음을 알 수 있었다.
- https://odd-one-out.serek.eu/projects/esp8266-nodemcu-dht22-mqtt-deep-sleep/

기존의 tmr.alarm으로 주기를 결정했었는데, 그냥 여기서는 node.dsleep(마이크로초)를 사용하여 주기를 결정하였다.일정시간 후 깨어나면 다시 wifi를 접속하고 데이터를 전송하는 방식이다. 여기서 주의할 점은 dsleep의 시간설정을 위해서 D0포트와RST포트를 연결해 주어야 한다는 것이다.
### 고정IP 사용하여 전력량을 줄이기

현재 wifi를 hybird egg를 사용하고 있는데 접속하면 DHCP 자동으로 IP를 할당받게 된다. 수동으로 IP를 설정할 수 있도록 한다.
## 2018.02.03

### MQTT 이용하여 thingspeak 전송하기

위와 같이 해놓고 보조배터리에 연결하고 퇴근을 했는데 하룻밤새에 또 방전이 되고 말았다. thingspeak에 메시지 전송시에 기존 server 접속방법이 아닌 iot 메시지 규격인 MQTT를 이용해본다.
- https://kr.mathworks.com/help/thingspeak/use-arduino-client-to-publish-to-a-channel.html
- thingspeak > account > MQTT API Key 생성

MQTT API Key를 이용하는 줄 알았는데 그대로 writekey를 이용하여 데이터를 업데이트 한다.
업로드는 정상적으로 하는 것을 확인.. 현재 온습도가 측정안되는 부분은 모듈이상으로 판단됨.

## 2018.02.05

### MQTT 전송 최적화

MQTT가 안되었던 것이 아니고 기존 데이터가 많아서 메모리가 부족했던 것으로 보인다. dht값을 100단위로 받아서 /,%로 소숫점을 표현했던 함수들이 메모리를 많이 잡은 것으로 판단됨. 기존처럼 dht.read(5)로 실행하니 정상동작함. 하지만 소숫점 표현이 불가함. firmware를 int -> float로 변경 후 정상 표현 됨 확인. 이제 배터리가 1달 이상 유지 되는 것이 관건임. 

## 2018.02.06

### LTE Egg 인터넷 끊기는 문제?

약 12시간 정도 동작을 하다가 온습도 수집이 멈췄다. 기존처럼 건전지가 방전된 줄 알았는데 보조배터리 전력량 체크를 보니 아직 4칸 가득 차 있다. 예상되는 상황은 두가지 이다. 하나는 LTE Egg가 약 24시간에 한번정도 세션을 초기화 하는 경우, 또 하나는 보드가 동작 중에 비정상적으로 행이 걸리는 경우이다. 아니면 두가지 상황이 복합적으로 발생한 것일 수 도 있다.

## 2018.02.07

### 매일 야간에 끊기는 원인 확인!!

오늘 새벽에도 온습도 측정이 멈춰있었다. 배터리는 아직 3칸이 차 있는 상황. 아무래도 LTE Egg가 24시간에 한번 세션을 초기화 해서인것 같았는데.. 소스코드를 보다 원인을 발견했다. 지난 2/2에 전력소모를 줄이기 위해서 고정IP를 할당했었는데 하기처럼 wifi 접속여부를 확인하지 않은체 다음 코드로 진행되게 된다. wifi.sta.getip()가 아닌 다른 방법으로 접속여부를 확인 하던지 동적IP할당으로 변경 해야한다.

```lua
if client_ip ~= "" then
    wifi.sta.setip({ip=client_ip,netmask=client_netmask,gateway=client_gateway})
end

tmr.alarm(1, 1000, 1, function()
    if wifi.sta.getip() == nil then
        print("Waiting for IP address...")
    else
```

위 방법을 해결 하기 위해 if절 을 변경하였다.

```lua
tmr.alarm(1, 1000, 1, function()
    if wifi.sta.status() ~= 1 then
        print("Waiting for IP address...")
```

### 펌웨어 업그레이드로 문제 해결!

해결이 된 줄 알았는데.. status가 처음에 단말을 켜도 1(IDLE)이다. 한번 서버에 접속을 시도 한 이후에 3으로 변경되는데 그 이후 다시 연결을 시도하질 못한다.

```lua
m:connect( "mqtt.thingspeak.com" , 1883, 0, 0, function(client)
        print("Connected to MQTT")
        print("  IP: mqtt.thingspeak.com")
        print("  Port: 1883")
        client:publish("channels/"..channelID.."/publish/"..writeKey,"field1="..temp.."&field2="..humi,0,0,function(client)
            print("Going to deep sleep for "..(time_between_sensor_readings/1000000).." seconds")
            node.dsleep(time_between_sensor_readings)    
        end)
    end,
    function(client, reason)
        print("failed reason: " .. reason)
    end)  
```

위 코드에서 connect 시도 실패 후 실패사유를 찍는 함수가 동작을 하면 거기서 재기동을 하면 되는데 그게 안되었다.
그런데 결국 Fimrware를 업그레이드 하니깐 된다!!! 올레

### 그런데 결국 wifi.sta.status() 의미가 없다.

init.lua에서 1차적으로 wifi 연결여부를 확인하지 않고 바로 thingspeak로 전송 하려고한다. 그러므로 init.lua의 if절은 필요없게된다. 하기 코드 없이 바로 startup()함수를 호출 시키도록 하였다. 

```lua
tmr.alarm(1, 1000, 1, function()
    if wifi.sta.status() ~= 1 then
        print("Waiting for IP address...")
    else
        tmr.stop(1)
        print("WiFi connection established, IP address: " .. wifi.sta.getip())
        print("Waiting...")
        tmr.alarm(0, 3000, 0, startup)
    end
end)
```

### wifi.sta.status() 대신에 wifi.sta.getrssi() 함수 사용

실제 wifi 접속여부를 확인하기 위해 getrssi를 사용하였다. 와이파이 신호를 잡기 전까지는 nil을 리턴한다! 드디어 해결했다!

### 최초 wifi 검색시 하염없이 찾는것을 방지하기

최초 wifi 검색 시 AP가 꺼져있거나 하는 문제가 발생하면 단말이 하염없이 WIFI를 잡느라 켜져있게 된다. 이를 방지하기 위해 30초 정도 기다려도 wifi 신호가 없으면 5분간 Deep Sleep에 들어가도록 하였다.

```lua
i = 1
tmr.alarm(1, 1000, 1, function()
    if wifi.sta.getrssi() == nil then
        if i == 30 then
            print("Going to deep sleep for 300 seconds")
            node.dsleep(300*1000*1000)
        end
        print("Waiting for IP address...")
        i = i + 1
```

## 2018.02.09

### 배터리 자동꺼짐 기능에 의한 문제

nodemcu의 전원공급을 보조배터리로 하고있었다. 그런데 최신 배터리(?)의 경우 자동꺼짐기능이 있어 단말 Deep Sleep기능 이후 깨어나지 못하는 문제가 발생했다. 보조배터리를 이미 10개 주문한 상태여서 Deep sleep이 아닌 다른 전원절약 방법을 선택해야한다. 2/2 사용하려다 실패한 nodemcu에 있는 node.sleep(), wifi.suspend()를 다시 사용해야한다
### Thingspeak 동일 채널에 동시에 logging 불가한 문제

thingspeak 하나의 채널에 여러개 IoT기기의 온습도 정보를 로깅하려고 하였다. 기존에 5분단위로 업로드할때는 문제가 되지 않았다. 그런데 위 문제(배터리 자동꺼짐)때문에 임시로 deepsleep을 사용하지 않고 10초단위로 데이터를 올리려고 하니 정상적으로 데이터가 업로드 되지 않는 문제가 있었다. 확인해 보니 동일 채널데이터에 데이터를 올릴때 제한이 있는 것 같다.

- http://community.thingspeak.com/forum/thingspeak-api/do-multiple-devices-require-multiple-channels/ : thingspeak 단일채널 접속 제한 관련 글

일단 채널을 전부 분리해 두었다. 추후 iot 기기가 안정화되고 간격도 5분정도로 길어지면 다시 동일 채널에 합치는 방법을 고려해야겠다.
## 2018.02.10

### node.sleep(), wifi.suspend() 기능을 사용하기 위한 firmware 다시 build하기

nodemcu firmware 기본기능 중 위 기능이 off되어있어 이를 on 시키기 위해서는 firmware 소스코드를 수정하고 재빌드를 해야한다.

- 변경위치: /app/include/user_config.h
- 변경내용: #define PMSLEEP_ENABLE 주석 제거

그 밖에 user_modules.h에서 필요한 기능을 활성화/비활성화 시킬 수 있다.

소스코드 수정 후 build하는 방법으로 docker를 활용할 수 있다. win10에 docker를 올리고 하려했으나 왜인지... 'docker is starting'이라고만 되어있고 진행이 되지 않는다. 임시방편으로 USB우분투에 docker를 설치하여 위 소스를 빌드하였다. 최신 빌드된 firmware를 기기에 적용하고 실행하니 suspend()기능이 사용 가능해졌다.

## 2018.02.12

### 배터리 교체의 필요성

위 LIGHT SLEEP은 사용은 가능해졌지만 주문한 배터리에 더 큰 문제가 있었다. SLEEP 여부와 상관없이 WIFI 전송을 하지 않으면 전류가 너무 약해서 배터리 충전을 멈춰버린다. 한번 배터리가 깨어나면 약 25~30초정도 유지를 하는데, 즉 그 시간안에 측정데이터를 WIFI를 이용해 전송을 해야 꺼지지 않는다. 배터리를 다시주문했다.. ㅜㅜ


## 2018.02.13

### Push 알람 구축 알아보기

일단 기본적인 온습도 측정시스템은 구축이 완료되었다. 추가적으로 실시간으로 알람을 보낼 수 있는 법을 찾고 있다. 방법은 두가지 정도로 생각하고 있다.
1. nodemcu에서 바로 push알람서버로 메시지 보내는 방법
- http://networkstarts.tistory.com/32: pushbullet으로 알람 보내기
2. thingspeak등의 사이트와 IFTTT를 연동해서 알람을 발생시키는 방법

## 2018.02.14

### thingspeak 일일 허용량 초과

thingspeak으로 mqtt 메시지가 전달이 안되고 계속해서 Broker로 부터 Timeout이 발생했다. Timeout이 점점 반복되더니 모든 nodemcu들이 MQTT 접속 자체가 불가해졌다. 무슨 특이사항이 있는 것인가 한참을 검토해본 결과 Thingspeak에서 허용하는 message 양을 초과 한 것을 알게 되었다. 배터리 문제로 인해 25초 간격으로 온/습도 메시지를 전달 한 것이 원인이었다.

- https://thingspeak.com/prices/thingspeak_home: 유료 계정 설명

유료 결제를 해야지만 그 이상 메시지가 발생 가능하다. 배터리 안정화 이후라고 하더라도 16개 단말이 3분 간격으로 메시지를 보내면 허용량을 초과하게 된다. 층 별로 계정을 나누어서 서비스를 제공해야 할 것 같다.

## 2018.02.16

### ThingHTTP, REACT 기능을 사용한 Pushbullet 알람 발생

nodemcu에서 바로 알람을 발생시킬경우 추가적인 배터리 사용이 있을 수 있고 코드 수정시 전체를 수정하는 번거로움이 있다. 그러던 도중 Thingspeak에서 바로 Pushbullet으로 Data를 연계하는 방법이 있다는 것을 알게 되었다.

- http://souliss.net/media/Souliss-ThingSpeaks-Pushbullet/ : ThingHTTP, REACT 기능을 사용하여 PushBullet으로 알람 연계하기
- https://binaryjam.com/2015/03/29/posting-to-pushbullet-via-thingspeak/ : 
- https://kr.mathworks.com/help/thingspeak/thinghttp-app.html : ThingHTTP에서 trigger Data 사용하기

테스트 해보니 특정 온도 초과시에 알람이 잘 발생한다. 알람 내용도 어느정도 customize 가능한데 Channel ID, 시각, 온도 정보를 뿌려줄 수 있다. 아직 Channel 이름을 연계하는 방법을 모르겠다. 그리고 시각 정보도 미국 기준으로 작성이 되는 것같다. Time 변경하는 법도 찾아 봐야한다. (시각은 Thingspeak 설정에서 Timezone 변경후 정상)

팀원 공유를 위해서 pushbullet Channel 을 공유 하는 방식을 사용하기로 했다. pushbullet에서 채널을 생성 한 후에 thingHTTP Post body에서 **Channel_tag=hlr**로 설정해 주면 해당 태그로 글을 올리게 된다. 그 후 Channel 공유 링크를 팀원들에게 주어 온습도 정보를 Follow 하게 하면 된다.

Thingspeak의 ThingHTTP에서 Channel이름을 그대로 릴레이해서 사용하고 싶었으나 그러한 변수가 없어서 각각의 온습도 채널마다 ThingHTTP를 생성하고 이름을 다르게 출력하게 하였다.

## 2018.02.18

### Thingspeak React 기능으로 베터리 이상 알람 추가

Thingspeak React중 **'No Data Check'** 라는 기능이 있다. 말 그대로 들어온 데이터가 없음을 체크하고 다음 행동을 설정하는 것이다. 이를 통해 30분 간격으로 10분동안 데이터 유무를 확인하여 배터리 리셋 또는 교체 알람을 발생하도록 하였다.

### -999도 예외처리
온도 정보 수집에러 발생시 (-999도) 전송하지 않도록 예외처리. 값이 너무 커서 그래프보기가 어려워진다.

## 2018.02.21

### 아이폰x에서 Pushbullet 알람 미수신 관련 문제

아이폰X에서 푸쉬알람이 수신되지 않는 문제가 있어 확인해보니 pushbullet이 ios 업데이트를 1년 넘게 하지 않고있다.

- <https://www.reddit.com/r/PushBullet/comments/7uix05/ios_update_iphone_x_support/>

다른 푸쉬알람에 대해 알아봐야 한다. 지금 구상중인 방법은 FCM/GCM (구글 푸쉬서비스)와 카카오톡 서비스를 잘 활용해서 알람 메시지를 카톡으로 받아보게 하는 것이다.

Thingspeak(HTTP POST) --> FCM(Firebase Cloud Message) --> KakaoTalk

## 2018.02.22

### 아이폰x에서 Pushbullet 알람 미수신 관련 문제(2) - Telegram

위 방법으로 진행 중 Telegram 어플이 좀 더 간편할 것 같아서 계획을 변경했다. 기존 ThingHTTP에서 Pushbullet에서 Telegram으로 변경하는 작업이 필요했다. 문제는 Pushbullet은 메시지를 올리는데 **application/json** 형식을 사용했는데, Telegram은 **application/x-www-form-urlencoded** 방식을 사용했다. 일부 문법에서 어려움이 있었다.

```
URL: https://api.telegram.org/bot[Telegram_API_Key]/sendMessage
Method: POST
Content Type: application/x-www-form-urlencoded
HTTP Version: 1.1
Host: api.telegram.org
Body: chat_id=[Telegram Group Channel ID]&text=<b>4F_0332_AAA#4507 배터리 알람</b> ID:%%channel_id%% 현재시각:%%datetime%% 현재온도:%%trigger%% &parse_mode=HTML
```

텔레그램의 Chat ID 획득에도 애를 먹었는데 우선 **getUpdates** 함수로 chat ID를 획득한다.
> https://api.telegram.org/bot[Telegram_API_Key]/getUpdates

그런데 개인적인 메시지를 수신할땐 해당 Bot과 채팅하는 ID를 입력하면 되는데 단체에서 사용하는 채널에서는 그게 안된다. 결국 **그룹생성**을 하여 거기에 텔레그램 Bot을 초대하고 해당 그룹채널 ID를 사용하는 방법으로 해결 했다.

## 2018.03.02

### 데스크탑 그래프 출력하기

배터리가 새로와서 총 15개의 IoT기기를 전부 설치했고 모바일 어플을 통한 모니터링 및 Push 기능은 완료되었다. 이제 실시간으로 컴퓨터에서 확인이 가능한 데스크탑용 모니터링이 필요하다. 우선 Thingspeak에 존재하는 기능은 2가지가 있다.

1. Matlab Visualizaiton -> Plot data from multiple fields 그래프 
> 하나의 그래프에 여러개의 채널ID_Field 표현이 가능하고 Public주소로 어디서든 접근이 가능하다. 그러나 그래프가 너무 작고 디자인 변경이 불가하다.

2. Plugin -> Chart With Multiple Series 그래프 (구글제공?)
> Javascript&HIGHCHART를 이용. 하나의 그래프의 여러개의 Field 표현이 가능하고 풍부한 표현이 가능하다. 그러나 로그인을 해야지만 해당 Plugin 실행이 가능하다.

위 두가지의 장점을 갖고있는 무언가를 찾다가. HTML에 Javascript와 HIGHCHART를 바로 실행해서 필요한 데이터를 갖고올 수 있다는 사실을 알게되었다.

- http://community.thingspeak.com/forum/announcements/thingspeak-live-chart-multi-channel-second-axis-historical-data-csv-export/ : 예시 중 1개
- https://www.highcharts.com/demo : highchart 소스코드 예제
- https://api.highcharts.com/highstock/labels.items : highchart 함수 사용 예제

## 2018.03.05

### 온습도 AVG,MAX 값 매일 1회 문자메시지 or 이메일로 통보하기

온습도기기의 일일 평균과 최대값을 계산하고 노티하는 기능을 추가하려고 한다. 알람은 즉각적으로 조치가 필요할 때 사용하고, AVG/MAX값은 해당 위치의 배풍기 조절 등이 필요한지 판단할 때 좋은 자료가 될것 같다. 우선 AVG, MAX값 계산은 Thingspeak의 analysis 기능을 활용한다. 하루 치 데이터를 바탕으로 max(최대값), mean(평균값)을 계산하고 Field에 등록하는 방식이다. 

```
ChannelID1 = 426739;
ChannelID2 = 426741;
ChannelID3 = 437128;
ChannelID4 = 437130;

writeAPIKey1 = '';
writeAPIKey2 = '';
writeAPIKey3 = '';
writeAPIKey4 = '';

temp1 = thingSpeakRead(ChannelID1,'Fields',1,'NumDays',1);
temp2 = thingSpeakRead(ChannelID2,'Fields',1,'NumDays',1);
temp3 = thingSpeakRead(ChannelID3,'Fields',1,'NumDays',1);
temp4 = thingSpeakRead(ChannelID4,'Fields',1,'NumDays',1);


maxTemp1 = max(temp1);
maxTemp2 = max(temp2);
maxTemp3 = max(temp3);
maxTemp4 = max(temp4);
 

display(maxTemp1)
display(maxTemp2)
display(maxTemp3)
display(maxTemp4)

thingSpeakWrite(ChannelID1,'Fields',3,'Values',maxTemp1,'writekey',writeAPIKey1);
thingSpeakWrite(ChannelID2,'Fields',3,'Values',maxTemp1,'writekey',writeAPIKey2);
thingSpeakWrite(ChannelID3,'Fields',3,'Values',maxTemp1,'writekey',writeAPIKey3);
thingSpeakWrite(ChannelID4,'Fields',3,'Values',maxTemp1,'writekey',writeAPIKey4);
```

여기에 한가지 문제가 있다. 동일 channel에 write 할때 15초의 딜레이가 있어야 하는데 min/max 필드가 기존 온/습도와 같은 channel을 사용하여 충돌이 발생 할 수 있다. 온습도기야 매번 재전송을 하니 큰 문제가 없지만, 위 정보는 하루에 한번 수행 할 것인데 온습도 정보등록 직후라면 실패를 하게된다.

그래서 생각한 해결방법! 바로 별도 channel을 생성 후 write를 할 때 한꺼번에 write를 하기로 했다. 그리고 또 짜잘한 문제가 몇가지 더 있었는데... 평균값을 사용할때 nan 값이 있으면 평균이 안나오는 문제, 소수점이 너무 많아 값이 길어지는 문제가 있었다. 각각 omitnan과 round 함수를 사용하여 해결 하였다.

```
ChannelID1 = 426739;
ChannelID2 = 426741;
ChannelID3 = 437128;
ChannelID4 = 437130;
ChannelID_AVG = 440303;
ChannelID_MAX = 440307;

writeAPIKey_AVG = '';
writeAPIKey_MAX = '';


temp1 = thingSpeakRead(ChannelID1,'Fields',1,'NumDays',1);
temp2 = thingSpeakRead(ChannelID2,'Fields',1,'NumDays',1);
temp3 = thingSpeakRead(ChannelID3,'Fields',1,'NumDays',1);
temp4 = thingSpeakRead(ChannelID4,'Fields',1,'NumDays',1);


maxTemp1 = max(temp1);
maxTemp2 = max(temp2);
maxTemp3 = max(temp3);
maxTemp4 = max(temp4);

avgTemp1 = round(mean(temp1,'omitnan'),1);
avgTemp2 = round(mean(temp2,'omitnan'),1);
avgTemp3 = round(mean(temp3,'omitnan'),1);
avgTemp4 = round(mean(temp4,'omitnan'),1);

thingSpeakWrite(ChannelID_MAX,'Fields',[1,2,3,4],'Values',[maxTemp1,maxTemp2,maxTemp3,maxTemp4],'writekey',writeAPIKey_MAX);
thingSpeakWrite(ChannelID_AVG,'Fields',[1,2,3,4],'Values',[avgTemp1,avgTemp2,avgTemp3,avgTemp4],'writekey',writeAPIKey_AVG);
```
