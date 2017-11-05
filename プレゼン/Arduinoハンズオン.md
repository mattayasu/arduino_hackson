<!-- $theme: gaia -->
<!-- page_number: true -->

Arduinoハンズオン
===

# ![](/Users/Yasuaki_air/Documents/GitHub/arduino_hackson/arduino-UNO.png)

###### Created by Yasuaki Haga

---
## Agenda

#### Target
Arduinoの概要について学び、簡単なコントロール方法について習得する

---

#### Agenda

- Arduinoとは
- GPIOの入出力操作方法
- PCとの通信方法
- アナログ入出力方法
- GROVE(紹介)
---

## Arduinoとは

https://www.slideshare.net/katsuhiromorishita/arduino-48588178

https://www.slideshare.net/ohwada/20140910-arduino

---

## スケッチ例

![](/Users/Yasuaki_air/Documents/GitHub/arduino_hackson/スケッチ_ex.png)
![](/Users/Yasuaki_air/Documents/GitHub/arduino_hackson/スケッチ_ex1.png)



---

## Lチカ

BLINKのテンプレートを使用して、Lチカをしてみよう。
(Lチカ=LEDをチカチカさせること。マイコン工作の基礎。プログラミングで言うところの"Hello world"を表示させること)

---

# GPIOの入出力方法

---

### GPIOの入出力方法（関数）

#### 使用する関数

##### 初期関数内
	pinMode(pin, mode) ;

##### loop関数内
    digitalWrite(pin, mode) ;
    digitalRead(pin)

---
**pinMode(pin, mode) ;**
pin: 設定したいピンの番号
mode: INPUT、OUTPUT、INPUT_PULLUP

**digitalWrite(pin, mode) ;**
pin: ピン番号
value: HIGHかLOW

**digitalRead(pin) ;**
pin: ピン番号
戻り値: HIGHかLOW

---
# 例題

2番PINを出力設定にし、LEDを接続。そのLEDを1秒間隔で点滅させてください

**回路図挿入**

---
# 時間があったら

時間があるようなら、下記をチャレンジ

1. LEDの点滅時間を変えてみる
2. 複数のLEDを制御

---
# タクトスイッチ
本日使うスイッチはタクトスイッチと呼ばれるものです。
タクトスイッチは以下のような構造になっています。
ボタンを押すと、通電する仕組みです。

![](/Users/Yasuaki_air/Documents/GitHub/arduino_hackson/タクトスイッチ.gif)



---
# 例題

7番PINを入力設定にし、スイッチを接続。スイッチが押されているときだけ、LEDを点灯させてください。

**回路図挿入**

---

# PCとの通信方法

---

### PCとの通信方法
PCのSerial Portを使い、Arduinoと通信することが可能。

#### 使用する関数

##### 初期関数内
	Serial.begin(speed)

##### loop関数内
	Serial.print(data, format)
    Serial.read()

---

**Serial.begin(speed)**
シリアル通信のデータ転送レート
speed: 転送レート (int) 
**Serial.print(data, format)**
シリアル出力
data: 出力する値。すべての型に対応しています。
format: 基数または有効桁数(浮動小数点数の場合) 
**Serial.read()**
受信データを読み込みます。
戻り値:読み込み可能なデータの最初の1バイトを返します。-1の場合は、データが存在しません 

---

```
//exmple//
void setup() {
	Serial.begin(9600);	// 9600bpsでポートを開く
}

void loop() {

　int read_val;
  Serial.print("loop start");       // 文字列を送信
}
 
 read_val = Serial.read();

  Serial.print("read val is %s\n", read_Val);       // 文字列を送信
}
```

---

# 例題

1. スイッチの状態をSerial Port上に表示してください
　スイッチが押されている状態 : "Switch ON!"
　スイッチが離されている状態 : "Switch OFF!"
2. PCからの命令でLEDを制御してください
　"1"が入力 : LED on
　"0"が入力 : LED off

---

# アナログ入出方法

---

### アナログ入出力方法

#### 使用する関数

##### 初期関数内
	必要なし

##### loop関数内
    analogWrite(pin, value) 
	analogRead(pin)

---
**analogWrite(pin, value)**
指定したピンからアナログ値(PWM波)を出力します。LEDの明るさを変えたいときや、モータの回転スピードを調整したいときに使える。

参考 : PWMはパルス信号のデューティー比を変化させて変調させる変調方法
![](/Users/Yasuaki_air/Documents/GitHub/arduino_hackson/pwm_sample2.png)

---
**analogWrite(pin, value)**
pin: 出力に使うピンの番号
value: デューティ比（0から255）
valueに0の時は、duty = 0, 255の時は、duty = 1.0となる。
```
//exmple
int ledPin = 9;      // LEDはピン9に接続
void setup() {
}
void loop() {
  analogWrite(ledPin, 0);	//duty=0 led消灯
  delay(1000);
  analogWrite(ledPin, 128);	//duty=0.5 led50%輝度 
  delay(1000);
  analogWrite(ledPin, 255);	//duty=1 led 100%輝度
}
```

---

# 例題
1.PWMを使用し、LEDの輝度を0%-100%-0%と段階的に変化させてください

## 時間が合ったら
1. PCからのコマンドで、LEDの明るさをコントロールしてください

---
**analogRead(pin)**
指定したアナログピンから値を読み取る。0から5ボルトの入力電圧を0から1023の数値に変換することができます。
pin: 読みたいピンの番号
戻り値 : 0から1023までの整数値
```
//example
int analogPin = 3;
int val = 0;
void setup() {
  Serial.begin(9600);        // シリアル通信の初期化
}
void loop() {
  val = analogRead(analogPin);    // アナログピンを読み取る
  Serial.println(val);
}
```

---

# 例題
1. 光センサーをアナログ入力とし、手をかざしたらled off、手を離したらledをonする回路を作成してください
2. 半固定抵抗を使用しアナログ入力pinにかかる電圧をコントロールして、LEDの明るさをコントロールしてください。

---

# GROVE
Arduinoでセンサやスイッチを簡単に扱えるのスターターキットです。はんだ付け不要でコントロール可能。各センサーに対して、ライブラリも揃っている。
![](/Users/Yasuaki_air/Documents/GitHub/arduino_hackson/grove4_s.png)


