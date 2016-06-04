#ArduinoML for Pharo

#DSL example
```Smalltalk
| myApp result |
myApp := #myApp arduinoApp
    bricks:
        {#button sensorOnPin: 9.
        #led actuatorOnPin: 12};
    states:{
        #offline stateDo: {#led setTo: #LOW}.
        #online stateDo: {#led setTo: #HIGH}};
    transitions:{
        #offline to: #online when: (#button is: #HIGH).
        #online to: #offline when: (#button is: #LOW).
        };
    build.
result := AMLArduinoCodeVisitor new visitApp: myApp.
result contents
```

generate : 

```C
int button = 9;
int led = 12;
void setup(){
	pinMode(button, INPUT);
	pinMode(led, OUTPUT);
}
void state_offline(){
	digitalWrite(led, LOW);
	if (digitalRead(button) == HIGH) {state_online();} else {state_offline();}
}
void state_online(){
	digitalWrite(led, HIGH);
	if (digitalRead(button) == LOW) {state_offline();} else {state_online();}
}
void loop(){
	state_offline();
}
```

##Install ArduinoML in Pharo 5.0 (Spur VM)
* Download a Spur VM: https://ci.inria.fr/pharo/view/5.0-VM-Spur/job/PharoVM-spur32/
* Download the last dev MOOSE 6.0 on INRIA's CI server: https://ci.inria.fr/moose/job/moose-6.0/
* Install GitFileTree from Configuration Browser
* Execute in a Playground:
```Smalltalk
Metacello new
    baseline: 'ArduinoML';
    repository: 'github://SergeStinckwich/ArduinoML-kernel/embeddeds/pharo';
    load
```
