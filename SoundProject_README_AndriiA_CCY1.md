
### Process - Research 
At first before I begin working on my project I went through the Examples section in the Sound Library to see how different code sequences affect the sound and visual output. This was a stage when I was just exploring the library and experimenting, seeing what is possible. For instance, filters such as BandPass Filter provide good examples of how to map out an output sound based on the user's position of the mouse, PeakAmplitude provides a good example of how sound amplitude can be visualised, Keyboard sketch provides good example of how key press commands can trigger the sound play. Sometimes when I was confused about certain pieces of code, I copied them or took screenshot and asked ChatGTP to explain me what a certain function does and this way I was able to analyse examples better and learn quicker. 
### Process - Setup and App Design 
The first variable I wanted to focus on is Volume Control as a fundamnetal element of Sound Design and how the user perceives the sound. 
I also want the user to be able to see the volume level visually
Then I want to add some key commands to regulate the amount of noise in my sound synthesizer to increase or decrease it 
### Process - Setup & Volume mapping 
So to manipulate volume I have to work with amp() or amplitude and connect it to my volume variable so that the values of the volume as a varibale will be equal to amplitude of the sound. That is the only way I know of how to change volume in the sketch. Amp() is a float parameter varibale so I will assign my volume variable to float type and its range will be from 0.0 ( complete silence ) to 1.0 ( full volume ). I then have to map it accordingly to the dimensions of my sketch but first I will create my varibales and setup. I used official documentation to get the right parameters for amp() : https://processing.org/reference/libraries/sound/SawOsc_amp_.html
```java 
import processing.sound.*; //importing the official sound library from Processing Foundation

SinOsc osc; // defining my oscilliator i use from the processing library for this project 
float volume; // setting up my volume variable as a float 
int volumeLevel; // volume level is the visual representation of the volume being played, int because i will use pixels on the screen to draw it

void setup() {
  size(800, 400); // size of the canvas
  volume = 0; // initial volume level to zero
  volumeLevel = 0; // initial volume level to zero
  osc = new SinOsc(this); // to be fair i still hardly understand why we use (this) in java but as i understand what we did in class this is a special word in java that basically helps us to use and change oscillator in the sketch 
  osc.freq(440); // 440 Hz would be the default frequncy for the sound in Oscillators
  osc.play(); // play the sound
}
```
Now to assign the volume level to my mouse posistion I need to use map() function, because my volumeLevel visual representation of the sound os horozintal, i will use x axis to map my volume level. If it was, however, vertical, for some reason, I would use y axis. https://processing.org/reference/map_.html . The mapping is familiar to me already as we used it in the processing class lectures, p5.js and arduino c++ codes, so i just map out the values as needed. 

```java 
void draw() {
  background(255); // i need to set a background color as a "first layer" of my sketch so that all the UI elements will go on top of it
  
  osc.amp(volume); // assigning the amplitude as volume level
  volume = map(mouseX, 0, width, 0, 1); // mapping volume - mouse X ( x asis ) as the incoming value to be converted, volume as float from 0.0 to 1.0 
```
### Process - Visual Volume Level 

Now I need to create a visual output illustration for the volume level; I will display it as a long rectangular volume bar similar to the default ones in the audio analysis and music software. The color I assign will be olive green; 
In terms of mapping I also have to map it as an integer becasue otherwise it will be interpreted as float and the visual output will be incorrect. When working with pixels it is important to map out values an integers. Processing also does not allow to map it as a float if the varibale is assigned as an integer, so i have to use int(map(();

```java
volumeLevel = int(map(volume, 0, 1, 0, width)); // mapping out the volumeLevel as integer for visual output
  fill(0, 100, 0); // setting the color of the volume bar to olive green 
  rect(0, height/2, volumeLevel, 20); // setting the bar as rect and giving it physicla parameters - position, height and width. I did not necceseraly wanted to center // it so it will be a bit below the center as well   
```
### Process - Formatting numbers as string in Processing, adding textbox output 

To make my app more informative I considered adding the textbox output that would show current volume level. This is the step that I had to stop on for a while but I managed to find a solution using nf() function that formats numbers into a string. https://processing.org/reference/nf_.html 

```java
fill(0); // black text 
  textSize(24); 
  textAlign(CENTER, CENTER); // centering the text 
  text("Volume Level: " + nf(volume, 1, 2), width/2, height/2 - 50); // nf(volume, 1, 2) basically means that there will be output according to my volume or amplitude 
// level, 1 digital on the right from the dot and two digits on the right, that is typical for float values so I used it in this case
```
### Process - Adding Noise to the sketch 
Now that the volume is set, I want to add a feature to my application that would increase or decrease the noise level to it if the user presses a certain key. I consider adding a similar visual representation to show the noise level. But I have to add the noise to my sketch and edit my setup accordingly.

```java
SinOsc osc;
WhiteNoise noise; // importing white noise from the sound library
float volume;
float noiseVolume; // defining the noise volume variable
int volumeLevel;
int noiseVolumeLevel; // defining visual representation of the noise varibale 
```
```java
void setup() {
  size(800, 400);
  volume = 0;
  noiseVolume = 0; // setting initial noise to zero
  volumeLevel = 0;
  noiseVolumeLevel = 0; // setting initial noise to zero
  osc = new SinOsc(this);
  noise = new WhiteNoise(this); // setting up white noise to be used in the sketch
  osc.freq(440);
  osc.play();
  noise.play(); // play noise
}
```
Now I have to map my noise amplitude the same way that I have done with the volume 

```java
noise.amp(noiseVolume); // assigning the noise amplitude to the noiseVolume varibale I defined

//mapping out the visual output of the noise level just like I did with the sound level 
noiseVolumeLevel = int(map(noiseVolume, 0, 1, 0, width));
  fill(100, 0, 0);
  rect(0, height/2 + 40, noiseVolumeLevel, 20);

//adding text to show noise level in my textbox
fill(0);
  textSize(24);
  textAlign(CENTER, CENTER);
  text("Sine Wave Volume Level: " + nf(volume, 1, 2), width/2, height/2 - 50);
  text("Noise Volume Level: " + nf(noiseVolume, 1, 2), width/2, height/2 + 90); // I position the noise text below volume text
```



void keyPressed() {
  if (key == 'q' || key == 'Q') {
    noiseVolume -= 0.1;
    if (noiseVolume < 0) {
      noiseVolume = 0;
    }
  } else if (key == 'w' || key == 'W') {
    noiseVolume += 0.1;
    if (noiseVolume > 1) {
      noiseVolume = 1;
    }
  }
}
