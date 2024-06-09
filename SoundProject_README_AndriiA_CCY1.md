
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

  
