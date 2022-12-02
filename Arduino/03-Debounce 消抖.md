## Debounce



### 按钮开关LED消抖代码

```
int flag;// 开关标志
int buttonPin = 3;// 按钮对应的引脚
int ledPin = 8;// LED对应的引脚
boolean buttonState = flase;// 按钮状态，初始状态为断开
vold setuf() {
	pinMode(buttonPin, INPUT);
	Serial.begin(9600);
	pinMode(ledPin, OUTPUT);
}

void loop() {
	flag = digitalRead(buttonPin);// 如果按钮按下，flag = 1
	Serial.println(flag);
	
	if(flag == 1) {
		buttonState = !buttonState;
		digitalWrite(ledPin, buttonState);
		delay(300); //延时300毫秒，防止按钮抖动，实现消抖
	}
}
```

以上代码实现了消抖，但是需要快速按下按钮再松开，但是如果长按按钮，也会导致无法确定是否能够实现按一下按钮切就换灯的状态，导致不稳定。

以下代码实现了长按按钮也不会导致功能不稳定：

```c
//要实现长按按钮但不切换功能，那就说明需要多监听一个状态：按钮是否抬起。上面的代码只监听了按钮是否按下
int flag;// 开关标志
int buttonPin = 3;// 按钮对应的引脚
int ledPin = 8;// LED对应的引脚
boolean buttonState = false;// 按钮是否接通，初始为false
boolean buttonUp = true;// 按钮是否抬起，初始为true
vold setuf() {
	pinMode(buttonPin, INPUT);
	Serial.begin(9600);
	pinMode(ledPin, OUTPUT);
}

void loop() {
    /*
    该循环的逻辑是，如果按下开关，判断到开关仍处于Up状态，切换开关接通状态、开关抬起状态，并点亮LED；
    直至松开开关后，恢复Up状态。
    */
	flag = digitalRead(buttonPin);
	Serial.println(flag);
	
	if(flag == 1 && buttonUp == 1) {
        buttonState = !buttonState;
        buttonUp = !buttonUp;
		digitalWrite(ledPin, buttonState);
	} else if(flag == 0 && buttonUp == 0) {
        buttonUp = !buttonUp;
    }
}

```

