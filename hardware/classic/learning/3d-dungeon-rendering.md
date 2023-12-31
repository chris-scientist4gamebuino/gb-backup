
# 3D dungeon rendering

[Back to Home](./../../../README.MD) | [Back to Learning for Classic](./README.MD) | [Go to Classic Reference Summary](./../reference/README.MD)

The following code demonstrates a simple 3D dungeon rendering engine on Gamebuino. A video of this demo in action can be found at [https://youtu.be/4m-Nj2IkDpo?si=jQJ0Y_40d0oIqcEG](https://youtu.be/4m-Nj2IkDpo?si=jQJ0Y_40d0oIqcEG)

```
// Gamebuino 3D rendering demo - by Mark Feldman (aka "Myndale" on the Gamebuino forums)
//
// PCD8544 initialization code based on examples at http://playground.arduino.cc/Code/PCD8544
//
// This code has been written to be relatively easy to follow but there's been virtually no attempt to optimize it whatsoever.
// If I were to optimize it I would start by rotating the back buffer 90 degrees and switching the LCD to the alternate
// mode where you write a full column at a time. Next I would hard-code functions to draw a line of wall pixels for each size
// that the wall can be drawn at, plus a jump table to call the appropriate handler. This would eliminate the need for a
// full-screen back-buffer altogether.
//
// The visible-surface detemination code needs an overhaul, it just draws rectangles of blocks from the camera cell out without
// much attempt to cull those outside the view frustum or behind other cells that have already been rendered.
//
// Finally the sprite draw routine could also do with a rewrite, the current version uses fixed-point arithmetic which is much
// faster to do in assembly than in C.

#include <SPI.h>
#include <Gamebuino.h>
#include <EEPROM.h>
#include <avr/pgmspace.h>
Gamebuino gb;

#define FOV  30
#define NEAR_PLANE (0.5/tan(PI*FOV/180))
#define CLIP_PLANE 0.01f
#define CAMERA_SCALE 50
#define WALL_HEIGHT 0.8f
#define OBJECT_SIZE 0.8f
#define BLOCKING_DIST 0.1f       // how close the player is allowed to walk up to walls
#define MOVEMENT 0.1f
#define TURN 0.1f

float xpos, zpos, lastx, lastz;
int xcell = 1;
int zcell = 1;
float dir = 0.0f;
float cos_dir;
float sin_dir;

extern uint8_t _displayBuffer[LCDWIDTH * LCDHEIGHT / 8];
float wbuffer[LCDWIDTH];
int numColumns;

// this is a very inefficient way of storing the map, it
// would be much better to store it with 1 cell per bit.
// that way you could easily store it in RAM and have
// bigger levels and more than 1.
#define MAP_SIZE 16
const char theMap[MAP_SIZE*MAP_SIZE+1] PROGMEM =
{
  "################"
  "#.#............#"
  "#.#..#.........#"
  "#.#..#.........#"
  "#....#....#....#"
  "##.#.#.........#"
  "######.........#"
  "#..............#"
  "##########..####"
  "#........#.....#"
  "#.....#..#..#..#"
  "#.....#.....#..#"
  "#.....##########"
  "#..............#"
  "#.#.#.#.#......#"
  "################"
};

#define SWAP(A, B) B, A,

// raw data for the monster sprite
const byte monster[] =
{
  SWAP(B00000000, B00000000)
  SWAP(B11111111, B11111000)
  SWAP(B01000000, B00001100)
  SWAP(B00101000, B00000110)
  SWAP(B01001100, B00000010)
  SWAP(B10000110, B01110011)
  SWAP(B01000010, B10001001)
  SWAP(B00100010, B10101001)
  SWAP(B01000010, B10001001)
  SWAP(B10000110, B01110011)
  SWAP(B01001100, B00000010)
  SWAP(B00101000, B00000110)
  SWAP(B01000000, B00001100)
  SWAP(B11111111, B11111000)
  SWAP(B00000000, B00000000)
  SWAP(B00000000, B00000000)
};

// the mask for the monster sprite indicating which pixels should be drawn
const byte monster_mask[] =
{
  SWAP(B00000000, B00000000)
  SWAP(B11111111, B11111000)
  SWAP(B01111111, B11111100)
  SWAP(B00111111, B11111110)
  SWAP(B01111111, B11111110)
  SWAP(B11111111, B11111111)
  SWAP(B01111111, B11111111)
  SWAP(B00111111, B11111111)
  SWAP(B01111111, B11111111)
  SWAP(B11111111, B11111111)
  SWAP(B01111111, B11111110)
  SWAP(B00111111, B11111110)
  SWAP(B01111111, B11111100)
  SWAP(B11111111, B11111000)
  SWAP(B00000000, B00000000)
  SWAP(B00000000, B00000000)
};

// raw data for the key sprite
const byte key[] =
{
  SWAP(B00000011, B10000000)
  SWAP(B00111110, B10000000)
  SWAP(B00010000, B10000000)
  SWAP(B00111110, B10000000)
  SWAP(B00000010, B10000000)
  SWAP(B00000010, B10000000)
  SWAP(B00000010, B10000000)
  SWAP(B00000110, B11000000)
  SWAP(B00001100, B01100000)
  SWAP(B00001000, B00100000)
  SWAP(B00001011, B10100000)
  SWAP(B00001010, B10100000)
  SWAP(B00001011, B10100000)
  SWAP(B00001000, B00100000)
  SWAP(B00001100, B01100000)
  SWAP(B00000111, B11000000)
};

// the mask for the monster sprite indicating which pixels should be drawn
const byte key_mask[] =
{
  SWAP(B00000011, B10000000)
  SWAP(B00111111, B10000000)
  SWAP(B00011111, B10000000)
  SWAP(B00111111, B10000000)
  SWAP(B00000011, B10000000)
  SWAP(B00000011, B10000000)
  SWAP(B00000011, B10000000)
  SWAP(B00000111, B11000000)
  SWAP(B00001111, B11100000)
  SWAP(B00001111, B11100000)
  SWAP(B00001111, B11100000)
  SWAP(B00001110, B11100000)
  SWAP(B00001111, B11100000)
  SWAP(B00001111, B11100000)
  SWAP(B00001111, B11100000)
  SWAP(B00000111, B11000000)
};

void setup(void) {
  gb.begin();
  initGame();  
}

void initGame() {
  gb.titleScreen(F("    3D DEMO\n\nControls:\n \25 strafe\n \26 run "));
  gb.display.persistence = false;
  gb.battery.show = false;
  
  lastx = xpos = 1.5f;
  lastz = zpos = 1.5f;
  dir = 0.0f;
}

// the loop routine runs over and over again forever
void loop(){
  if(gb.update())
  {
    if(gb.buttons.pressed(BTN_C)){
      initGame();
    }
    
    boolean strafe = (digitalRead(BTN_A_PIN)==LOW);
    float movement = MOVEMENT;
    float turn = TURN;
    if (digitalRead(BTN_B_PIN)==LOW)
    {
      movement *= 2.0f;
      turn *= 2.0f;
    }
    
    if (digitalRead(BTN_DOWN_PIN)==LOW)
    {
      xpos -= movement * sin_dir;
      zpos -= movement * cos_dir;
    }
    
    if (digitalRead(BTN_UP_PIN)==LOW)
    {
      xpos += movement * sin_dir;
      zpos += movement * cos_dir;
    }
    
    if (digitalRead(BTN_LEFT_PIN)==LOW)
    {
      if (strafe)
      {
        xpos -= movement * cos_dir;
        zpos += movement * sin_dir;
      }
      else
        dir -= turn;
    }
    
    if (digitalRead(BTN_RIGHT_PIN)==LOW)
    {
      if (strafe)
      {
        xpos += movement * cos_dir;
        zpos -= movement * sin_dir;
      }
      else
        dir += turn;
    }
    
    xpos = max(xpos, 0);
    zpos = max(zpos, 0);
    xpos = min(xpos, MAP_SIZE);
    zpos = min(zpos, MAP_SIZE);
    
    // don't let the player walk through walls    
    if ((lastx != xpos) || (lastz != zpos))
    {
      int cellX = (int)lastx;
      int cellZ = (int)lastz;
      if ((!isValid(cellX-1, cellZ) || isBlocked(cellX-1, cellZ)) && ((xpos-cellX)<BLOCKING_DIST))
        xpos = cellX + BLOCKING_DIST;
      if ((!isValid(cellX+1, cellZ) || isBlocked(cellX+1, cellZ)) && ((1-xpos+cellX)<BLOCKING_DIST))
        xpos = cellX + 1 - BLOCKING_DIST;
      if ((!isValid(cellX, cellZ-1) || isBlocked(cellX, cellZ-1)) && ((zpos-cellZ)<BLOCKING_DIST))
        zpos = cellZ + BLOCKING_DIST;
      if ((!isValid(cellX, cellZ+1) || isBlocked(cellX, cellZ+1)) && ((1-zpos+cellZ)<BLOCKING_DIST))
        zpos = cellZ + 1 - BLOCKING_DIST;
    }
    lastx = xpos;
    lastz = zpos;
    
    drawFrame();
  }
}

boolean isValid(int cellX, int cellZ)
{
  if (cellX < 0)
    return false;
  if (cellX >= MAP_SIZE)
    return false;
  if (cellZ < 0)
    return false;
  if (cellZ >= MAP_SIZE)
    return false;
  return true;
}

boolean isBlocked(int cellX, int cellZ)
{
  return pgm_read_byte(theMap + cellZ * MAP_SIZE + cellX) == '#';
}

void drawFrame()
{
  cos_dir = cos(dir);
  sin_dir = sin(dir);
  xcell = floor(xpos);
  zcell = floor(zpos);
  initWBuffer();
  drawFloorAndCeiling();  
  for (int layer=1; (layer<MAP_SIZE) && (numColumns<LCDWIDTH); layer++)
    drawLayer(layer);
  drawSprite(3.5f, 1.5f, monster, monster_mask);
  drawSprite(3.5f, 11.5f, key, key_mask);
}

void initWBuffer()
{
  for (int i=0; i<LCDWIDTH; i++)
    wbuffer[i] = 0.0f;
  numColumns = 0;
}

void drawFloorAndCeiling()
{
  memset(_displayBuffer, 0xff, 3*84);
  for (int y=3, ofs=3*84; y<6; y++)
    for (int x=0; x<84; x+=2)
    {
      _displayBuffer[ofs++] = 0x55;
      _displayBuffer[ofs++] = 0xaa;
    }
}

void drawLayer(int layer)
{
  drawCell(xcell, zcell-layer);
  drawCell(xcell, zcell+layer);
  drawCell(xcell-layer, zcell);
  drawCell(xcell+layer, zcell);
  for (int i=1; i<layer; i++)
  {
    drawCell(xcell-i, zcell+layer);
    drawCell(xcell+i, zcell-layer);
    drawCell(xcell-layer, zcell-i);
    drawCell(xcell+layer, zcell+i);
  }
  for (int i=1; (i<layer) && (numColumns<LCDWIDTH); i++)
  {
    drawCell(xcell+i, zcell+layer);
    drawCell(xcell-i, zcell-layer);
    drawCell(xcell-layer, zcell+i);
    drawCell(xcell+layer, zcell-i);
  }
  drawCell(xcell-layer, zcell+layer);
  drawCell(xcell+layer, zcell+layer);
  drawCell(xcell-layer, zcell-layer);
  drawCell(xcell+layer, zcell-layer);
}

void drawCell(int cellX, int cellZ)
{
  if (!isValid(cellX, cellZ))
    return;
  if (!isBlocked(cellX, cellZ))
    return;
  if (zpos < cellZ)
  {
    if (xpos > cellX)
    {
      // north west quadrant
      if ((zpos < cellZ) && !isBlocked(cellX, cellZ-1))
        drawWall(cellX, cellZ, cellX+1, cellZ);  // south wall
      if ((xpos > cellX+1) && (!isBlocked(cellX+1, cellZ)))
        drawWall(cellX+1, cellZ, cellX+1, cellZ+1);  // east wall
    }
    else
    {
      // north east quadrant
      if ((zpos < cellZ) && !isBlocked(cellX, cellZ-1))
        drawWall(cellX, cellZ, cellX+1, cellZ);  // south wall
      if ((xpos< cellX) && !isBlocked(cellX-1, cellZ))
        drawWall(cellX, cellZ+1, cellX, cellZ);  // west wall
    }
  }
  else
  {
    if (xpos > cellX)
    {
      // south west quadrant
      if ((zpos > cellZ+1) && !isBlocked(cellX, cellZ+1))
        drawWall(cellX+1, cellZ+1, cellX, cellZ+1);  // north wall
      if ((xpos > cellX+1) && !isBlocked(cellX+1, cellZ))
        drawWall(cellX+1, cellZ, cellX+1, cellZ+1);  // east wall
    }
    else
    {
      // south east quadrant
      if ((zpos > cellZ+1) && !isBlocked(cellX, cellZ+1))
        drawWall(cellX+1, cellZ+1, cellX, cellZ+1);  // north wall
      if ((xpos< cellX) && !isBlocked(cellX-1, cellZ))
        drawWall(cellX, cellZ+1, cellX, cellZ);  // west wall
    }
  }
}

// look-up table to speed up calculation of the line address
int y_lut[48] = {0,0,0,0,0,0,0,0,84,84,84,84,84,84,84,84,168,168,168,168,168,168,168,168,252,252,252,252,252,252,252,252,336,336,336,336,336,336,336,336,420,420,420,420,420,420,420,420};

inline void setPixel(int x, int y)
{
  _displayBuffer[y_lut[y] + x] |= (0x01 << (y & 7));
}

inline void clearPixel(int x, int y)
{
  _displayBuffer[y_lut[y] + x] &= ~(0x01 << (y & 7));
}

// draws one side of a cell
void drawWall(float _x1, float _z1, float _x2, float _z2)
{
  // find position of wall edges relative to eye
  float x1 = cos_dir * (_x1-xpos) - sin_dir * (_z1-zpos);
  float z1 = sin_dir * (_x1-xpos) + cos_dir * (_z1-zpos);
  float x2 = cos_dir * (_x2-xpos) - sin_dir * (_z2-zpos);
  float z2 = sin_dir * (_x2-xpos) + cos_dir * (_z2-zpos);
  
  // clip to the front pane
  if ((z1<CLIP_PLANE) && (z2<CLIP_PLANE))
    return;
  if (z1 < CLIP_PLANE)
  {
    x1 += (CLIP_PLANE-z1) * (x2-x1) / (z2-z1);
    z1 = CLIP_PLANE;
  }
  else if (z2 < CLIP_PLANE)
  {
    x2 += (CLIP_PLANE-z2) * (x1-x2) / (z1-z2);
    z2 = CLIP_PLANE;
  }
  
  // apply perspective projection
  float vx1 = x1 * NEAR_PLANE * CAMERA_SCALE / z1;  
  float vx2 = x2 * NEAR_PLANE * CAMERA_SCALE / z2; 
  
  // transform the end points into screen space
  int sx1 = (int)ceil((LCDWIDTH / 2) + vx1);
  int sx2 = (int)ceil((LCDWIDTH / 2) + vx2) - 1;
  
  // clamp to the visible portion of the screen
  int firstx = max(sx1, 0);
  int lastx = min(sx2, LCDWIDTH-1);
  if (lastx < firstx)
    return;
  
  // loop across each column
  float w1 = 1.0f / z1;
  float w2 = 1.0f / z2;
  float delta_w = (w2 - w1) / (sx2-sx1);
  float w = w1 + (firstx - sx1) * delta_w;
  for (int x=firstx; x<=lastx; x++, w+=delta_w)
  {
    if (w > wbuffer[x])
    {        
      wbuffer[x] = w;
      numColumns++;
      
      // calculate top and bottom
      float vy = (WALL_HEIGHT / 2.0) * NEAR_PLANE * CAMERA_SCALE * w;
      int sy1 = (int)ceil(LCDHEIGHT / 2 - vy);
      int sy2 = (int)ceil(LCDHEIGHT / 2 + vy) - 1;
  
      // clamp to the visible portion of the screen
      int firsty = max(sy1, 0);
      int lasty = min(sy2, LCDHEIGHT-1);
      
      // draw this column
      if ((x==sx1) || (x==sx2))
        for (int y=firsty; y<=lasty; y++)
          setPixel(x, y);
        else
          for (int y=firsty; y<=lasty; y++)
            clearPixel(x, y);
      if (sy1 >= 0)
        setPixel(x, sy1);
      if (sy2 < LCDHEIGHT)
        setPixel(x, sy2);
    }
  }
}

// I have done a little bit of optimization here, namely the use of 4:12 fixed-point arithmetic.
void drawSprite(float _x, float _z, const byte * sprite, const byte * mask)
{
  // find position relative to eye
  float x = cos_dir * (_x-xpos) - sin_dir * (_z-zpos);
  float z = sin_dir * (_x-xpos) + cos_dir * (_z-zpos);
  
  // make sure it's in front of us
  if (z < CLIP_PLANE)
    return;
  float w = 1.0f / z;

  // project the mid-point onto the screen
  float midx = x * NEAR_PLANE * CAMERA_SCALE * w;
  float screen_size = 0.5f * OBJECT_SIZE * CAMERA_SCALE * w;
  int sx1 = (int)ceil(LCDWIDTH / 2 + midx - screen_size);
  int sx2 = (int)ceil(LCDWIDTH / 2 + midx + screen_size) - 1;
  int sy1 = (int)ceil(LCDHEIGHT / 2 - screen_size);
  int sy2 = (int)ceil(LCDHEIGHT / 2 + screen_size) - 1;
  
  int firstx = max(sx1, 0);
  int lastx = min(sx2, LCDWIDTH-1);
  if (firstx > lastx)
    return;
  int firsty = max(sy1, 0);
  int lasty = min(sy2, LCDHEIGHT-1);
  
  unsigned deltax = 0x10000 / (sx2-sx1+1);
  unsigned deltay = 0x10000 / (sy2-sy1+1);
  unsigned first_srcy = 0x10000 * (firsty-sy1) / (sy2-sy1+1);
  unsigned srcx = 0x10000 * (firstx-sx1) / (sx2-sx1+1);
  for (int sx=firstx; sx<=lastx; sx++, srcx+=deltax)
    if (w >= wbuffer[sx])
    {      
      unsigned line = ((unsigned *)sprite)[srcx>>12];
      unsigned line_mask = ((unsigned *)mask)[srcx>>12];
      unsigned srcy = first_srcy;
      for (int sy=firsty; sy<=lasty; sy++, srcy+=deltay)
      {
        unsigned bit_mask = 1<<(srcy>>12);
        if (line_mask & bit_mask)
        {
          if (line & bit_mask)
            setPixel(sx, sy);
          else
            clearPixel(sx, sy);
        }
      }
    }
}
```

## General information

Saved on September 30, 2023
