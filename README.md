# 470 project 2

# Description
This project consists of 3 buttons, 3 sliding switches, and a potentiometer.

One button can produce a sine (low) or square (high) wave, another can produce a square (low) or sawtooth (high) wave, and the last can produce a sawtooth (low) or sine (high) wave.

Press/hold the buttons to produce sound, use the slide switches to change which waveform each button produces, and use the potentiometer to determine the pitch of the sound produced.

There is a slight delay between when the input is received and the output is heard, but this is most likely due to the serial port being slow.

The main idea behind the project was trying to make a musical instrument, but a really janky and bad one.

This project uses the p5.js serial library to handle serial input.

# Arduino code

<code>
    
    const int dialPin = A0;
    const int buttonPinYellow = 2;
    const int buttonPinRed = 5;
    const int buttonPinWhite = 7;
    const int switchPinYellow = 6;
    const int switchPinRed = 8;
    const int switchPinWhite = 9;
    int anVal = 0;
    String output = "";

    int buttonYellowState = 0;
    int buttonRedState = 0;
    int buttonWhiteState = 0;

    int switchYellowState = 0;
    int switchRedState = 0;
    int switchWhiteState = 0;

    void setup() {
        pinMode(buttonPinYellow,INPUT);
        pinMode(buttonPinRed,INPUT);
        pinMode(buttonPinWhite,INPUT);
        pinMode(switchPinYellow,INPUT);
        pinMode(switchPinRed,INPUT);
        pinMode(switchPinWhite,INPUT);

        Serial.begin(9600);
    }

    void loop() {
        anVal = analogRead(dialPin);

        buttonYellowState = digitalRead(buttonPinYellow);
        buttonRedState = digitalRead(buttonPinRed);
        buttonWhiteState = digitalRead(buttonPinWhite);

        switchYellowState = digitalRead(switchPinYellow);
        switchRedState = digitalRead(switchPinRed);
        switchWhiteState = digitalRead(switchPinWhite);

        output += anVal;
        output += ",";
        if(buttonYellowState == HIGH){
          output += "1";
        }
        else{
          output += "0";
        }
        output += ",";
            if(buttonRedState == HIGH){
          output += "1";
        }
        else{
          output += "0";
        }
        output += ",";
            if(buttonWhiteState == HIGH){
          output += "1";
        }
        else{
          output += "0";
        }
        output += ",";
        if(switchYellowState == HIGH){
          output += "1";
        }
        else{
          output += "0";
        }
        output += ",";
            if(switchRedState == HIGH){
          output += "1";
        }
        else{
          output += "0";
        }
        output += ",";
            if(switchWhiteState == HIGH){
          output += "1";
        }
        else{
          output += "0";
        }

        Serial.println(output);
        output = "";
        delay(13);
    }
</code>

# P5 Code
<code>
    
    let serial;
    let portName = 'COM3';
    let inputString;

    let splitString;

    let yellowOscillators;
    let redOscillators;
    let whiteOscillators;

    function setup() {
      serial = new p5.SerialPort();
      serial.open(portName);
      yellowOscillators = [new p5.Oscillator('sine'), new p5.Oscillator('square')];
      redOscillators = [new p5.Oscillator('square'),new p5.Oscillator('sawtooth')];
      whiteOscillators = [new p5.Oscillator('sawtooth'),new p5.Oscillator('sine')];

      yellowOscillators[0].start();
      yellowOscillators[1].start();
      redOscillators[0].start();
      redOscillators[1].start();
      whiteOscillators[0].start();
      whiteOscillators[1].start();
    }


    function draw(){
      inputString = serial.readLine(portName);
      console.log(inputString);

      splitString = split(inputString,',');

      yellowOscillators[0].freq(int(splitString[0]));
      yellowOscillators[1].freq(int(splitString[0])*1.5);
      redOscillators[0].freq(int(splitString[0]));
      redOscillators[1].freq(int(splitString[0])*1.5);
      whiteOscillators[0].freq(int(splitString[0]));
      whiteOscillators[1].freq(int(splitString[0])*1.5);

      if(splitString[1] == '1'){
        if(splitString[4] == '1'){
          yellowOscillators[0].amp(1,0.1);
          yellowOscillators[1].amp(0,0.1);
        }
        else{
          yellowOscillators[0].amp(0,0.1);
          yellowOscillators[1].amp(1,0.1);
        }
      }
      else{
          yellowOscillators[0].amp(0,0.1);
          yellowOscillators[1].amp(0,0.1);
      }
      if(splitString[2] == '1'){
        if(splitString[5] == '1'){
          redOscillators[0].amp(1,0.1);
          redOscillators[1].amp(0,0.1);
        }
        else{
          redOscillators[0].amp(0,0.1);
          redOscillators[1].amp(1,0.1);
        }
      }
      else{
          redOscillators[0].amp(0,0.1);
          redOscillators[1].amp(0,0.1);
      }
      if(splitString[3] == '1'){
        if(splitString[6] == '1'){
          whiteOscillators[0].amp(1,0.1);
          whiteOscillators[1].amp(0,0.1);
        }
        else{
          whiteOscillators[0].amp(0,0.1);
          whiteOscillators[1].amp(1,0.1);
        }
      }
      else{
          whiteOscillators[0].amp(0,0.1);
          whiteOscillators[1].amp(0,0.1);
      }
    }
</code>
