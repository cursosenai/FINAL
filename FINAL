/*                                                SENAI CAXIAS
 *                                     ANALIZADOR DE COMBUSTIVEL ADULTERADO
 *                                                18/04/2024
 *                                                  ALUNOS
 *                                  ALEXANDRE, DANIEL, SILVANIA, LUCAS, BRENDA 
 */


#include <Wire.h>
#include <LiquidCrystal_I2C.h>		
#include <SoftwareSerial.h>

LiquidCrystal_I2C lcd(0x27,16,4);	    //Inicializa o display no endereco 0x27

SoftwareSerial bt(1,0);		            // RX, TX 
int sensor =A0;
int comb;
float combustivel;

//int pin = 10
int analyzer=0, comb1=0;
volatile int state = LOW;
volatile int state1 = LOW;

void analy()
{
analyzer++;
} 

void setup()
{

// inicializa Bluetooth 
bt.begin(9600);                     //Inicia a comunicação serial com o computador
Serial.begin(9600);                  //Inicia a comunicação serial com o modulo bluetooth  
  
//pinMode(pin, OUTPUT);
attachInterrupt(0, analy, FALLING);
 lcd.init();				                  //inicia LCD de 16 colunas 4 linhas
 lcd.setBacklight(HIGH);
 lcd.setCursor(0,0);			            //posiciona cursor na linha 1 coluna 2
 lcd.print("SENAI CAXIAS/RJ");		    //escreve no lcd na posição acima
 lcd.setCursor(0,1);			            //posiciona cursor na linha 1 coluna 2
 lcd.print("AUT. INDUSTRIAL");		    //escreve no lcd na posição acima
 delay(4000);
 lcd.clear();

// inicializa timer1 
  noInterrupts();           		      // desabilita todas as interrupções
  TCCR1A = 0;
  TCCR1B = 0;
  TCNT1  = 0;

  OCR1A = 31250;            		      // compare match register 16MHz/256/2Hz
  TCCR1B |= (1 << WGM12);   		      // CTC mode
  TCCR1B |= (1 << CS12);    		      // 256 prescaler 
  TIMSK1 |= (1 << OCIE1A);  		      // enable timer compare interrupt
  interrupts();             		      // enable all interrupts
  lcd.clear();        

}

ISR(TIMER1_COMPA_vect)          	    // timer compare interrupt service routine
{
  comb1=analyzer*2-50;  
  analyzer=0;
 }


void loop()
{
 
 lcd.setCursor(0,0);			            //posiciona cursor na linha 1 coluna 2
 lcd.print("PERCENTUAL DE");		      //escreve no lcd na posição acima
  lcd.setCursor(0,1);			            //posiciona cursor na linha 1 coluna 1
 lcd.print("ETANOL= ");			          //escreve no lcd na posição acima
 if(comb1>2){
 lcd.print(comb1);			              //escreve no lcd na posição acima 
lcd.print("%       ");			          //escreve no lcd na posição acima  
 }else{
  lcd.print("S/DADOS");			          //escreve no lcd na posição acima 
 }
 if(comb1>100){
  lcd.setCursor(4,2);			            //posiciona cursor na linha 1 coluna 2
  lcd.print("ERROR");
 }else{
   lcd.setCursor(4,2);			          //posiciona cursor na linha 1 coluna 2
 lcd.print("        ");
}

  bt.print(comb1);
  bt.println("%");
  Serial.println(comb1);
    
delay(1000);

}
