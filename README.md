/*
* Voltage_Monitor_with_Buzzer.ino
*
* Author: ChatGPT (modificado por você)
* Date: 18.03.2025
*
* Object: Medir voltagem com sensor de voltagem (até 25V), mostrar no LCD e acionar buzzer se voltagem <= 10V
*/

#include <LCD-I2C.h>
#include <Wire.h>

LCD_I2C lcd(0x27, 16, 2); // Endereço do LCD
const int buzzerPin = 8; // Pino do buzzer
const int voltagePin = A0; // Pino analógico para o sensor de voltagem

// Definindo a escala para o cálculo da voltagem
const float maxVoltage = 25.0; // Tensão máxima que o sensor pode medir (25V)
const int adcMaxValue = 1023;  // Valor máximo do ADC (10 bits: 0-1023)

void setup() {
    Wire.begin();
    lcd.begin(&Wire);
    lcd.display();
    lcd.backlight();
    
    pinMode(buzzerPin, OUTPUT); // Configura o pino do buzzer como saída
    pinMode(voltagePin, INPUT); // Configura o pino de leitura de voltagem como entrada
}

void loop() {
    // Lê o valor do sensor de voltagem (valor analógico entre 0 e 1023)
    int sensorValue = analogRead(voltagePin);
    
    // Converte o valor lido para a voltagem real (até 25V)
    float voltage = (sensorValue / float(adcMaxValue)) * maxVoltage;

    // Exibe o valor da voltagem no LCD
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Voltagem: ");
    lcd.print(voltage, 2); // Exibe a voltagem com 2 casas decimais
    lcd.print(" V");
    
    // Se a voltagem for menor ou igual a 10V, ativa o buzzer
    if (voltage <= 10.0) {
        tone(buzzerPin, 1000); // Emite som a 1000Hz
    } else {
        noTone(buzzerPin); // Desliga o buzzer
    }
    
    delay(1000); // Espera 1 segundo antes de ler a voltagem novamente
}
