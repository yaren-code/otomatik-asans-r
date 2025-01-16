int sensor1 = 2;  // 3. kat sensörü
int sensor2 = 3;  // 2. kat sensörü
int sensor3 = 4;  // 1. kat sensörü
int motorin1 = 5; // Motor yön kontrolü
int motorin2 = 6; // Motor yön kontrolü
int enablea = 9;  // Motor hız kontrolü (PWM)
int playPin = 7;  // ISD1820 PLAY pini

void setup() {
    pinMode(sensor1, INPUT);
    pinMode(sensor2, INPUT);
    pinMode(sensor3, INPUT);
    pinMode(motorin1, OUTPUT);
    pinMode(motorin2, OUTPUT);
    pinMode(enablea, OUTPUT);
    pinMode(playPin, OUTPUT);
    digitalWrite(playPin, LOW); // PLAY pini başlangıçta LOW
    stopMotor(); // Başlangıçta motor durduruluyor
}

void loop() {
    // Sensörlerin durumunu oku
    bool isSensor1Active = digitalRead(sensor1) == LOW; // 3. kat
    bool isSensor2Active = digitalRead(sensor2) == LOW; // 2. kat
    bool isSensor3Active = digitalRead(sensor3) == LOW; // 1. kat

    if (isSensor1Active) { 
        // 3. kat algılandıysa 1. kata in
        delay(2000); // 2 saniye bekleme
        moveMotorDown(); // Motor aşağı hareket eder
        delay(12000);    // 3. kattan 1. kata iniş süresi
        stopMotor();     // Motor durdurulur
        playSound();     // Ses çalınır
    } 
    else if (isSensor2Active) {
        // 1. kat algılandıysa 2. kata çık
        delay(2000); // 2 saniye bekleme
        moveMotorUp(); // Motor yukarı hareket eder
        delay(6000);   // 1. kattan 2. kata çıkış süresi
        stopMotor();   // Motor durdurulur
        playSound();   // Ses çalınır
    } 
    else if (isSensor3Active) {
        // 2. kat algılandıysa 3. kata çık
        delay(2000); // 2 saniye bekleme
        moveMotorUp(); // Motor yukarı hareket eder
        delay(6000);   // 2. kattan 3. kata çıkış süresi
        stopMotor();   // Motor durdurulur
        playSound();   // Ses çalınır
    }
}

// Motorun yukarı hareket etmesi için fonksiyon
void moveMotorUp() {
    digitalWrite(motorin1, HIGH);
    digitalWrite(motorin2, LOW);
    analogWrite(enablea, 150); // Motor hızı
}

// Motorun aşağı hareket etmesi için fonksiyon
void moveMotorDown() {
    digitalWrite(motorin1, LOW);
    digitalWrite(motorin2, HIGH);
    analogWrite(enablea, 150); // Motor hızı
}

// Motoru durdurmak için fonksiyon
void stopMotor() {
    digitalWrite(motorin1, LOW);
    digitalWrite(motorin2, LOW);
    analogWrite(enablea, 0); // Hızı sıfırla
}

// Ses çalmak için fonksiyon
void playSound() {
    digitalWrite(playPin, HIGH); // Ses çalmayı başlat
    delay(500);                 // Sesin başlaması için kısa süre bekle
    digitalWrite(playPin, LOW); // PLAY pinini LOW yap
}
