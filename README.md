- Работает как Switch, Dimmer, Number, Text 
 - В настройках меги указываем айпи сервера 192.168.0.1:8989 
 - имя скрипта - любое
 

 
 ### Чтобы изменить номер порта втавляем эти строчки в конец конфига Openhub:
```
################################ Megadevice Binding #######################################
#
# IP address of a Http port for megadevice defaut value is 8989
megadevice:httpserverport=8585
```
Для получения значений АЦП используем Number

Для получения температуры:

```
{megadevice="sec:192.168.0.17:0,dht11,t"}
```
т.е. после айпи адреса указываем через запятую порт, тип датчика и желаемый ответ
##### t- температура
##### h- влажность. (для 1w этого параметра нет)
типы датчиков, выставляются аналогично меги
##### dht11
##### dht22
##### 1w

### в OpenHAB *.items:
```
Switch MegaDeviceButton_kitchen 	"Kitchen button" {megadevice="sec:192.168.0.17:0"}
Switch KitchenLamp "Свет над кухней" (Hall, Hall_Lights) {megadevice="sec:192.168.0.17:9"}

Dimmer DimmedLight	"Dimmer [%d %%]"	{megadevice="sec:localhost:0"}

```
```
{megadevice="пароль на мегу : айпишник меги : номер порта меги"}
```
#### Пример Mega.rules:
```
rule "MegadeviceKitchenButtonPress"
when Item MegaDeviceButton_kitchen changed to ON
then if (KitchenLamp.state == OFF) {
	postUpdate(KitchenLamp, ON)
	sendCommand(KitchenLamp, ON)
} else {
	postUpdate(KitchenLamp, OFF)
	sendCommand(KitchenLamp, OFF)
}
end

rule "MegaDeviceBellButtonPress"
when Item MegaDeviceBell_Button changed to ON
then
	postUpdate(Bell, ON)
	sendCommand(Bell, ON)
end

rule "MegaDeviceBellButtonRelease"
when Item MegaDeviceBell_Button changed to OFF
then
	postUpdate(Bell, OFF)
	sendCommand(Bell, OFF)
end
```
### Пример с таймером:
```
var int Timeout

rule "Test"
when Item Megadevice_button changed to ON then
 Timeout = now.getMillisOfDay
 logInfo("StartTime", Timeout.toString)
end

rule "Test1"
when Item Megadevice_button changed to OFF then
var int Stop = now.getMillisOfDay
 logInfo("StopTime", Timeout.toString)
 
 var int result = Stop - Timeout
 
 result = result / 1000
 
 if (result <= 1) {
 	logInfo("Timer 1 second ", result.toString)
 	 if (HallLamp.state == OFF){ postUpdate(HallLamp,ON) } else {postUpdate(HallLamp,OFF)}
 	}
 	else if (result > 1 && result < 3){
 		postUpdate(HallLamp,OFF)
 	} else {
 		logInfo("Timer >3 seconds", result.toString)
 	}
end
```
### Пример default.siemaps:
```
sitemap default label= "Main Screen"{
	Frame label="Кухня" {
		Switch item=KitchenLamp
		Slider item=DimmedLight
	}
}
```

 - Работает как Switch, Dimmer, Number, Text 
 - В настройках меги указываем айпи сервера 192.168.0.1:8989 
 - имя скрипта - любое
 
 
 ### Чтобы изменить номер порта втавляем эти строчки в конец конфига Openhub:
```
################################ Megadevice Binding #######################################
#
# IP address of a Http port for megadevice defaut value is 8989
megadevice:httpserverport=8585



### в OpenHAB *.items:
```
Switch MegaDeviceButton_kitchen 	"Kitchen button" {megadevice="sec:192.168.0.17:0"}
Switch KitchenLamp "Свет над кухней" (Hall, Hall_Lights) {megadevice="sec:192.168.0.17:9"}

Dimmer DimmedLight	"Dimmer [%d %%]"	{megadevice="sec:localhost:0"}

```
```
{megadevice="пароль на мегу : айпишник меги : номер порта меги"}
```
#### Пример Mega.rules:
```
rule "MegadeviceKitchenButtonPress"
when Item MegaDeviceButton_kitchen changed to ON
then if (KitchenLamp.state == OFF) {
	postUpdate(KitchenLamp, ON)
	sendCommand(KitchenLamp, ON)
} else {
	postUpdate(KitchenLamp, OFF)
	sendCommand(KitchenLamp, OFF)
}
end

rule "MegaDeviceBellButtonPress"
when Item MegaDeviceBell_Button changed to ON
then
	postUpdate(Bell, ON)
	sendCommand(Bell, ON)
end

rule "MegaDeviceBellButtonRelease"
when Item MegaDeviceBell_Button changed to OFF
then
	postUpdate(Bell, OFF)
	sendCommand(Bell, OFF)
end
```
### Пример с таймером:
```
var int Timeout

rule "Test"
when Item Megadevice_button changed to ON then
 Timeout = now.getMillisOfDay
 logInfo("StartTime", Timeout.toString)
end

rule "Test1"
when Item Megadevice_button changed to OFF then
var int Stop = now.getMillisOfDay
 logInfo("StopTime", Timeout.toString)
 
 var int result = Stop - Timeout
 
 result = result / 1000
 
 if (result <= 1) {
 	logInfo("Timer 1 second ", result.toString)
 	 if (HallLamp.state == OFF){ postUpdate(HallLamp,ON) } else {postUpdate(HallLamp,OFF)}
 	}
 	else if (result > 1 && result < 3){
 		postUpdate(HallLamp,OFF)
 	} else {
 		logInfo("Timer >3 seconds", result.toString)
 	}
end
```
### Пример default.siemaps:
```
sitemap default label= "Main Screen"{
	Frame label="Кухня" {
		Switch item=KitchenLamp
		Slider item=DimmedLight
	}
}
```

