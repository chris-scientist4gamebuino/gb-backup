
# SD card audio streaming

[Back to Home](./../../../README.MD) | [Back to Learning for Classic](./README.MD) | [Go to Classic Reference Summary](./../reference/README.MD)

The following code will stream an audio file off the SD card and play it out the Gambuino speaker. The comments at the top of the source file explain how to convert any regular audio file (WAV, MP3 etc) into a stream of raw bytes ready for play.

A video of this demo in action can be found at [http://youtu.be/4ZI_CQeKIio](http://youtu.be/4ZI_CQeKIio)

```
// Gamebuino sound streaming sample - by Mark Feldman (aka "Myndale")
//
// Requires TimerOne library from http://playground.arduino.cc/Code/Timer1
//
// This demo will look for a file called SAMPLE.RAW on the SD card containing raw
// 8-bit signed samples intended to be played back at a frequency of 20kHz.
// These files can be easily generated using the ffmpeg utility (which can be
// downloaded at http://www.ffmpeg.org/) using the following command line parameters:
//
// ffmpeg -i your_sound_file.mp3 -f s8 -acodec pcm_s8 -ar 20000 -ac 1 -af "volume=5dB" SAMPLE.RAW


#include <TimerOne.h>
#include <SD.h>

#define PLAYBACK_FREQ 20000
#define SPEAKER 3
#define BUFFER_SIZE 128
unsigned char buffers[2][BUFFER_SIZE];
volatile int currentBuffer = 0;
volatile unsigned char * currentPtr;
volatile unsigned char * endPtr;
File dataFile;
const int chipSelect = 10;

void setup() {  
  pinMode(10, OUTPUT);  
  if (!SD.begin(chipSelect))
    return;
  dataFile = SD.open("SAMPLE.RAW");
  if (dataFile) {
    dataFile.read((void *)&buffers[0], BUFFER_SIZE);
    currentBuffer = 0;
    currentPtr = buffers[currentBuffer];
    endPtr = currentPtr + BUFFER_SIZE;
    pinMode(SPEAKER, OUTPUT);
    TCCR2B = (TCCR2B & 0b11111000) | 0x01;  // set max PWM frequency
    Timer1.initialize(1000000/PLAYBACK_FREQ);
    Timer1.attachInterrupt(callback);
  }  
}

void loop() {
  dataFile.read(&buffers[1], BUFFER_SIZE);
  while (currentBuffer != 1);
  dataFile.read(&buffers[0], BUFFER_SIZE);
  while (currentBuffer != 0);
}

void callback()
{
  analogWrite(SPEAKER, (*currentPtr++) + 128);
  if (currentPtr >= endPtr)
  {
    currentBuffer = 1-currentBuffer;
    currentPtr = buffers[currentBuffer];
    endPtr = currentPtr + BUFFER_SIZE;
  }
}
```

## General information

Saved on September 30, 2023
