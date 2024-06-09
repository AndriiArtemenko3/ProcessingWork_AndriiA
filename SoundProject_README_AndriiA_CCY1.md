
### Process - Research 
At first before I begin working on my project I went through the Examples section in the Sound Library to see how different code sequences affect the sound and visual output. This was a stage when I was just exploring the library and experimenting, seeing what is possible. For instance, filters such as BandPass Filter provide good examples of how to map out an output sound based on the user's position of the mouse, PeakAmplitude provides a good example of how sound amplitude can be visualised, Keyboard sketch provides good example of how key press commands can trigger the sound play. Sometimes when I was confused about certain pieces of code, I copied them or took screenshot and asked ChatGTP to explain me what a certain function does and this way I was able to analyse examples better and learn quicker. 
### Process - Setup and App Design 
The first variable I wanted to focus on is Volume Control as a fundamnetal element of Sound Design and how the user perceives the sound. 
I also want the user to be able to see the volume level visually
Then I want to add some key commands to regulate the amount of noise in my sound synthesizer to increase or decrease it 
### Process - Volume mapping 
So to manipulate volume I have to work with amp() or amplitude and connect it to my volume variable so that the values of the volume as a varibale will be equal to amplitude of the sound. That is the only way I know of how to change volume in the sketch. Amp() is a float parameter varibale so I will assign my volume variable to float type and its range will be from 0.0 ( complete silence ) to 1.0 ( full volume ). I then have to map it accordingly to the dimensions of my sketch but first I will create my varibales and setup. I used official documentation to get the right parameters for amp() : https://processing.org/reference/libraries/sound/SawOsc_amp_.html
'''java 
import processing.sound.*;

SinOsc osc;
float volume;
int volumeLevel;

void setup() {
  size(800, 400);
  volume = 0;
  volumeLevel = 0;
  osc = new SinOsc(this);
  osc.freq(440);
  osc.play();
}
'''
