/*
 * File: TetrisGame.ino
 * Author: Jacob Eriksson
 * Date: 2023-12-14
 * Description: This program runs a tetris-game along with the signature theme and a score-counter
 */

/*
 * Inkluderar nödvändiga bibliotek
 */
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <LedControl.h>

/*
 * Definerar värden som används för OLED-display
 */
#define WIDTH 64
#define HEIGHT 128
#define OLED_RESET -1
#define SCREEN_ADDRESS 0x3C
#define WHITE SSD1306_WHITE

/*
 * Definerar noter
 */
#define NOTE_B0  31
#define NOTE_C1  33
#define NOTE_CS1 35
#define NOTE_D1  37
#define NOTE_DS1 39
#define NOTE_E1  41
#define NOTE_F1  44
#define NOTE_FS1 46
#define NOTE_G1  49
#define NOTE_GS1 52
#define NOTE_A1  55
#define NOTE_AS1 58
#define NOTE_B1  62
#define NOTE_C2  65
#define NOTE_CS2 69
#define NOTE_D2  73
#define NOTE_DS2 78
#define NOTE_E2  82
#define NOTE_F2  87
#define NOTE_FS2 93
#define NOTE_G2  98
#define NOTE_GS2 104
#define NOTE_A2  110
#define NOTE_AS2 117
#define NOTE_B2  123
#define NOTE_C3  131
#define NOTE_CS3 139
#define NOTE_D3  147
#define NOTE_DS3 156
#define NOTE_E3  165
#define NOTE_F3  175
#define NOTE_FS3 185
#define NOTE_G3  196
#define NOTE_GS3 208
#define NOTE_A3  220
#define NOTE_AS3 233
#define NOTE_B3  247
#define NOTE_C4  262
#define NOTE_CS4 277
#define NOTE_D4  294
#define NOTE_DS4 311
#define NOTE_E4  330
#define NOTE_F4  349
#define NOTE_FS4 370
#define NOTE_G4  392
#define NOTE_GS4 415
#define NOTE_A4  440
#define NOTE_AS4 466
#define NOTE_B4  494
#define NOTE_C5  523
#define NOTE_CS5 554
#define NOTE_D5  587
#define NOTE_DS5 622
#define NOTE_E5  659
#define NOTE_F5  698
#define NOTE_FS5 740
#define NOTE_G5  784
#define NOTE_GS5 831
#define NOTE_A5  880
#define NOTE_AS5 932
#define NOTE_B5  988
#define NOTE_C6  1047
#define NOTE_CS6 1109
#define NOTE_D6  1175
#define NOTE_DS6 1245
#define NOTE_E6  1319
#define NOTE_F6  1397
#define NOTE_FS6 1480
#define NOTE_G6  1568
#define NOTE_GS6 1661
#define NOTE_A6  1760
#define NOTE_AS6 1865
#define NOTE_B6  1976
#define NOTE_C7  2093
#define NOTE_CS7 2217
#define NOTE_D7  2349
#define NOTE_DS7 2489
#define NOTE_E7  2637
#define NOTE_F7  2794
#define NOTE_FS7 2960
#define NOTE_G7  3136
#define NOTE_GS7 3322
#define NOTE_A7  3520
#define NOTE_AS7 3729
#define NOTE_B7  3951
#define NOTE_C8  4186
#define NOTE_CS8 4435
#define NOTE_D8  4699
#define NOTE_DS8 4978
#define REST 0

/*
 * Definerar variabeln lc som LedControl för 8-digit, 7-segment display
 */
LedControl lc=LedControl(12,13,10,1);

/*
 * Definerar OLED-displayens x-, och y-värden samt input och reset
 */
Adafruit_SSD1306 display(128, 64, &Wire, OLED_RESET);

/*
 * Array som bestämmer ordning av noter och pauser
 */
int melody[] = {
  NOTE_E5, 4,  NOTE_B4,8,  NOTE_C5,8,  NOTE_D5,4,  NOTE_C5,8,  NOTE_B4,8, NOTE_A4, 4,  NOTE_A4,8,  NOTE_C5,8,  NOTE_E5,4,  NOTE_D5,8,  NOTE_C5,8,
  NOTE_B4, -4,  NOTE_C5,8,  NOTE_D5,4,  NOTE_E5,4, NOTE_C5, 4,  NOTE_A4,4,  NOTE_A4,8,  NOTE_A4,4,  NOTE_B4,8,  NOTE_C5,8,
  NOTE_D5, -4,  NOTE_F5,8,  NOTE_A5,4,  NOTE_G5,8,  NOTE_F5,8, NOTE_E5, -4,  NOTE_C5,8,  NOTE_E5,4,  NOTE_D5,8,  NOTE_C5,8,
  NOTE_B4, 4,  NOTE_B4,8,  NOTE_C5,8,  NOTE_D5,4,  NOTE_E5,4,
  NOTE_C5, 4,  NOTE_A4,4,  NOTE_A4,4, REST, 4,
  NOTE_E5, 4,  NOTE_B4,8,  NOTE_C5,8,  NOTE_D5,4,  NOTE_C5,8,  NOTE_B4,8,
  NOTE_A4, 4,  NOTE_A4,8,  NOTE_C5,8,  NOTE_E5,4,  NOTE_D5,8,  NOTE_C5,8,
  NOTE_B4, -4,  NOTE_C5,8,  NOTE_D5,4,  NOTE_E5,4,
  NOTE_C5, 4,  NOTE_A4,4,  NOTE_A4,8,  NOTE_A4,4,  NOTE_B4,8,  NOTE_C5,8,
  NOTE_D5, -4,  NOTE_F5,8,  NOTE_A5,4,  NOTE_G5,8,  NOTE_F5,8,
  NOTE_E5, -4,  NOTE_C5,8,  NOTE_E5,4,  NOTE_D5,8,  NOTE_C5,8,
  NOTE_B4, 4,  NOTE_B4,8,  NOTE_C5,8,  NOTE_D5,4,  NOTE_E5,4,
  NOTE_C5, 4,  NOTE_A4,4,  NOTE_A4,4, REST, 4,
  NOTE_E5,2,  NOTE_C5,2,
  NOTE_D5,2,   NOTE_B4,2,
  NOTE_C5,2,   NOTE_A4,2,
  NOTE_GS4,2,  NOTE_B4,4,  REST,8, 
  NOTE_E5,2,   NOTE_C5,2,
  NOTE_D5,2,   NOTE_B4,2,
  NOTE_C5,4,   NOTE_E5,4,  NOTE_A5,2,
  NOTE_GS5,2,
};
/*
 * Definerar variabler som används för uppspelning av tetris-theme
 */
int tempo=144; 
int buzzer = 11;
int notes = sizeof(melody)/sizeof(melody[0])/2; 
int wholenote = (60000 * 4) / tempo; //bestämmer tempo i ms
int divider = 0;
int noteDuration = 0;

/*
 * Boolean som används för att se om griden är correct
 */
boolean grid[10][18];
//Integers som används för att måla grid och block
const int MARGIN_TOP = 19;
const int MARGIN_LEFT = 3;
const int SIZE = 5;
//Anger antalet typer av block
const int TYPES = 6;

/*
 * Integers för inputs
 */
int moveLeft = 4;
int moveRight = 6;
int fallSpeed = 5;
int rotatePiece = 7;

/*
 * Anger ord/variabler/booleans som används under koden
 */
word currentType, nextType, rotation;
int pieceX, pieceY;
int piece[2][4];
int pause = 400;
long rotateDelay = 0;
long timer = 0;
long score = 0;
boolean b1, b2, b3;

/*
 * 6 styckna arrayer som representerar varsin tetris-piece
 */
const char piece_l[2][2][4]{{
    {0, 1, 2, 3}, {0, 0, 0, 0}
  },
  {
    {0, 0, 0, 0}, {0, 1, 2, 3}
  }
};

const char piece_L_l[4][2][4]{{
    {0, 0, 0, 1}, {0, 1, 2, 2}
  },
  {
    {0, 1, 2, 2}, {1, 1, 1, 0}
  },
  {
    {0, 1, 1, 1}, {0, 0, 1, 2}
  },
  {
    {0, 0, 1, 2}, {1, 0, 0, 0}
  }
};

const char piece_sq[1][2][4]{{
    {0, 1, 0, 1}, {0, 0, 1, 1}
  }
};

const char piece_T[4][2][4]{{
   {0, 0, 1, 0},{0, 1, 1, 2}
  },
  {
    {0, 1, 1, 2},{1, 0, 1, 1}
  },
  {
    {1, 0, 1, 1},{0, 1, 1, 2}
  },
  {
    {0, 1, 1, 2},{0, 0, 1, 0}
  }
};

const char piece_S_l[2][2][4]{{
    {0, 0, 1, 1}, {0, 1, 1, 2}
  },
  {
    {0, 1, 1, 2}, {1, 1, 0, 0}
  }
};

const char piece_S_r[2][2][4]{{
    {1, 1, 0, 0}, {0, 1, 1, 2}
  },
  {
    {0, 1, 1, 2}, {0, 0, 1, 1}
  }
};

/*
 * Boolean som testar om piecen kan roteras
 */
boolean rotateAble(int rotation){
  int piece[2][4];
  copyOnePiece(piece, currentType, rotation);
  return(!horizontalCollision(piece, 0));
}

/*
 * Boolean som testar om piecen krockar då den rör sig horizontelt
 */
boolean horizontalCollision(int piece[2][4], int amount){
  for(int i = 0; i < 4; i++){
    int newX = pieceX + piece[0][i] + amount;
    if(newX > 9 || newX < 0 || grid[newX][pieceY + piece[1][i]])
      return true;
  }
  return false;
}
/*
 * Boolean som testar om piecen krockar då den rör sig vertikalt
 */
boolean verticalCollision(){
  for(int i = 0; i < 4; i += 1){
    int y = pieceY + piece[1][i] + 1;
    int x = pieceX + piece[0][i];
    if(y > 17 || grid[x][y])
      return true;
  }
  return false;
}
/*
 * Tar in piecens typ och ger tillbaka hur många sätt den kan roteras
 */
int maxRotation(int type){
  if(type == 0 || type == 4 || type == 5)
    return 2;
  else if(type == 1 || type == 3)
    return 4;
  else if(type == 2)
    return 1;
  else
    return 0;
}

void setup() {
  Wire.begin(); //Startar upp I2C portarna
  /*
   * Anger portar för styr-knappar
   */
  pinMode(moveLeft, INPUT_PULLUP);
  pinMode(moveRight, INPUT_PULLUP);
  pinMode(fallSpeed, INPUT_PULLUP);
  pinMode(rotatePiece, INPUT_PULLUP);
  Serial.begin(9600);
  if(!display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS)) { //Om addressen inte kan nås skickas felmeddelande (Address 0x3D för 128x64)
      Serial.println(F("SSD1306 allocation failed"));
      for(;;); //loopar för evigt
    }
  /*
   * Startar upp 8-digit, 7-segment display genom att bestämma läge och att rensa displayen
   */
  lc.shutdown(0,false);
  lc.setIntensity(0,8);
  lc.clearDisplay(0);
  display.setRotation(1); //Bestämmer rotation för OLED-displayen
  display.clearDisplay(); //Rensar OLED-displayen
  createLayout();
  display.display();
  randomSeed(analogRead(0));
  nextType = random(TYPES); //Genererar typen av nästa piece
  generateNextPiece();
  timer = millis(); //Sätter timer till samma värde som antalet millisekunder som gått sen start
  /*
   *  Spelar tetris-themet utifrån arrayen av noter
   */
  for (int thisNote = 0; thisNote < notes * 2; thisNote = thisNote + 2) {
      divider = melody[thisNote + 1];
    if (divider > 0) {
      noteDuration = (wholenote) / divider;
    } else if (divider < 0) {
      noteDuration = (wholenote) / abs(divider);
      noteDuration *= 1.5;
    }
    tone(buzzer, melody[thisNote], noteDuration * 0.9);
    delay(noteDuration);
    noTone(buzzer);
  }
}

void loop() { //Loop som körs hela tiden
  /*
   * Checkar om det har gått rätt mäng tid sen senaste fallet
   * Tar bort fulla linjer och uppdaterar skärmen
   * Checkar om fallandet av piecen kommer leda till en vertikal krock
   * Om det blir en vertikal krock stannar piecen och en ny genereras
   * Om det inte blir en vertikal krock ökar dess y-värde med +1, vilket leder till att den faller ett steg
   * Sätter timer = millis() för att (millis() - timer) ska vara lika med 0
   */
  if((millis() - timer) > pause){
    checkLine();
    refreshScreen();
    if(verticalCollision()){
      for(short i = 0; i < 4; i += 1)
        grid[pieceX + piece[0][i]][pieceY + piece[1][i]] = 1;
      generateNextPiece();
    }
    else{
      pieceY += 1;
    }
    timer = millis();
  }
  /*
   * Checkar om knappen för att flytta åt vänster är nedtryckt
   * Sätter boolean b1 som true
   * Ändrar piecens x-värde med -1 om piecen inte krockar
   * Sätter boolean b1 som false, vilket gör att piecen bara flyttas en gång
   */
  if(digitalRead(moveLeft) == LOW){
    delay(50);
    if(b1){
      if(!horizontalCollision(piece, -1)){
        pieceX -= 1;
        refreshScreen();
      }
      b1 = false;
    }
  }
  else{
    b1 = true;
  }
  /*
   * Checkar om knappen för att flytta åt höger är nedtryckt
   * Sätter boolean b2 som true
   * Ändrar piecens x-värde med +1 om piecen inte krockar
   * Sätter boolean b2 som false, vilket gör att piecen bara flyttas en gång
   */
  if(digitalRead(moveRight) == LOW){
    delay(50);
    if(b2){
      if(!horizontalCollision(piece, 1)){
        pieceX += 1;
        refreshScreen();
      }
      b2 = false;
    }
  }
  else{
    b2 = true;
  }
  /*
   * Checkar om knappen för att öka fallhastighet är nedtryckt
   * Om knappen är nedtryckt sänks fallspeed till 50 millisekunder, annars är den kvar på 1000 millisekunder
   */
  if(digitalRead(fallSpeed) == LOW){
    pause = 50;
  }
  else{
    pause = 1000;
  }
  /*
   * Checkar om knappen för att rotera piece är nedtryckt
   * Sätter boolean b3 som true
   * Om piecen inte kan rotera så stannar dess rotation på 0
   * Om piecen kan rotera ökar dess rotation med +1
   * Kopierar piecen med dess nya värde och uppdaterar displayen
   * Sätter rotateDelay = millis() för att (millis() - rotateDelay) ska vara lika med 0
   * Sätter boolean b3 som false då tiden gått över delayen, vilket gör att piecen bara roteras en gång
   */
  if(digitalRead(rotatePiece) == LOW){
    delay(50);
    if(b3){
      if((rotation == maxRotation(currentType) - 1) && (rotateAble(0))){
        rotation = 0;
      }
      else if(rotateAble(rotation + 1)){
        rotation += 1;
      }
      copyOnePiece(piece, currentType, rotation);
      refreshScreen();
      b3 = false;
      rotateDelay = millis();
    }
  }
  else if((millis() - rotateDelay) > 50){
    b3 = true;
  }
}

/*
 * Uppdaterar displayen
 */
void refreshScreen(){ 
   display.clearDisplay();
   createLayout();
   drawGrid();
   drawPiece(currentType, 0, pieceX, pieceY);
   display.display();
}

/*
 * Målar ramen för spelet
 */
void createLayout(){ 
  display.drawLine(0, 15, WIDTH, 15, WHITE);
  display.drawRect(0, 0, WIDTH, HEIGHT, WHITE);
  drawNextPiece();
}

/*
 * Går igenom alla celler i griden och målar en kvadrat i de celler som ska vara fyllda
 */
void drawGrid(){ 
  for(short x = 0; x < 10; x += 1)
    for(short y = 0; y < 18; y += 1)
      if(grid[x][y])
        display.fillRect(MARGIN_LEFT + (SIZE + 1)*x, MARGIN_TOP + (SIZE + 1)*y, SIZE, SIZE, WHITE);
}

/*
 * Går igenom alla rader och ser om varje cell i raden är fylld
 */
void checkLine(){ 
  boolean full;
  for (int y = 17; y >= 0; y -= 1){
    full = true;
    for(int x = 0; x < 10; x += 1){
      full = full && grid[x][y];
    }
      if(full){
        breakLine(y);
      y += 1;
    }
  }
}

/*
 * Tar bort raden vars celler alla är fyllda och låter ovanstående rader falla ned ett steg, ökar även 
 */
void breakLine(int line){ 
  for(int y = line; y >= 0; y -= 1){
    for(int x = 0; x < 10; x += 1){
      grid[x][y] = grid[x][y-1];
    }
  }
    for(int x = 0; x < 10; x += 1){
      grid[x][0] = 0;
    }
  display.invertDisplay(true);
  delay(50);
  display.invertDisplay(false);
  score += 1000;
  scoreCounter();
}

/*
 * Anger en integer och dess värde för varje digit i 8-digit, 7-segment displayen motsvarande dess plats i integern score
 * Displayar varje integer i dess motsvarande plats på 8-digit, 7-segment displayen
 */
void scoreCounter() {
  int num8 = (score / 1) % 10;
  int num7 = (score / 10) % 10;
  int num6 = (score / 100) % 10;
  int num5 = (score / 1000) % 10;
  int num4 = (score / 10000) % 10;
  int num3 = (score / 100000) % 10;
  int num2 = (score / 1000000) % 10;
  int num1 = (score / 10000000) % 10;

  lc.clearDisplay(0);
  lc.setDigit(0, 0, num8, false);
  lc.setDigit(0, 1, num7, false);
  lc.setDigit(0, 2, num6, false);
  lc.setDigit(0, 3, num5, false);
  lc.setDigit(0, 4, num4, false);
  lc.setDigit(0, 5, num3, false);
  lc.setDigit(0, 6, num2, false);
  lc.setDigit(0, 7, num1, false);
}

/*
 * Anger piecen sparad i integern nextType som den aktiva piecen
 * Slumpar fram en ny piece och sätter den som nextType, samt anger dess x- och y-kordinater och rotation
 */
void generateNextPiece(){
  currentType = nextType;
  nextType = random(TYPES);
  if(currentType != 5)
    pieceX = random(9);
  else
    pieceX = random(7);
  pieceY = 0;
  rotation = 0;
  copyOnePiece(piece, currentType, rotation);
}

/*
 * Målar piecen sparad i integern nextType uppe i högra hörnet
 */
void drawNextPiece(){
  int nextPiece[2][4];
  copyOnePiece(nextPiece, nextType, 0);
  for(int i = 0; i < 4; i += 1)
    display.fillRect(50 + 3*nextPiece[0][i], 4 + 3*nextPiece[1][i], 2, 2, WHITE);
}

/*
 * Målar den aktiva piecen
 */
void drawPiece(int type, int rotation, int x, int y){
  for(int i = 0; i < 4; i += 1)
    display.fillRect(MARGIN_LEFT + (SIZE + 1)*(x + piece[0][i]), MARGIN_TOP + (SIZE + 1)*(y + piece[1][i]), SIZE, SIZE, WHITE);
}

/*
 * Kopierar en piece utifrån dess typ samt rotation
 */
void copyOnePiece(int piece[2][4], int type, int rotation){
  if(type == 0)
    for(int i = 0; i < 4; i += 1){
      piece[0][i] = piece_l[rotation][0][i];
      piece[1][i] = piece_l[rotation][1][i];
    }
  else if(type ==  1)
    for(int i = 0; i < 4; i += 1){
      piece[0][i] = piece_L_l[rotation][0][i];
      piece[1][i] = piece_L_l[rotation][1][i];
    }
  else if(type == 2)
    for(int i = 0; i < 4; i += 1){
      piece[0][i] = piece_sq[rotation][0][i];
      piece[1][i] = piece_sq[rotation][1][i];
    }
  else if(type == 3)
    for(int i = 0; i < 4; i += 1){
      piece[0][i] = piece_T[rotation][0][i];
      piece[1][i] = piece_T[rotation][1][i];
    }
  else if(type == 4)
    for(int i = 0; i < 4; i += 1){
      piece[0][i] = piece_S_l[rotation][0][i];
      piece[1][i] = piece_S_l[rotation][1][i];
    }
  else if(type == 5)
    for(int i = 0; i < 4; i += 1){
      piece[0][i] = piece_S_r[rotation][0][i];
      piece[1][i] = piece_S_r[rotation][1][i];
    }
}
