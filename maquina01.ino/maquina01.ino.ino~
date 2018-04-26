/*
* MAQUINA DE VENDAS
*
* ANDERSON, JONATAN, LEANDRO, CESAR
*/

#include <Wire.h> 
#include <LiquidCrystal_I2C.h>

//VARIAVEIS GLOBAIS
int saldoUsuario=0;
int qtdMoeda100=0;
int qtdMoeda50=0;
int qtdMoeda25=0;
int qtdProduto1=5;
int qtdProduto2=5;
int qtdProduto3=5;
int qtdProduto4=5;
int estado = 0;
int devolve=0;
unsigned long getMillis=0; // mudei de int para long pq millis pode ser maior que o valor de int
int timer;
int relogio;//usado na devolução de moedas

//ENTRADAS
#define MOEDA_IN_100  5
#define MOEDA_IN_50  4
#define MOEDA_IN_25 3
#define PROD_IN_1 13
#define PROD_IN_2 12
#define PROD_IN_3 11
#define PROD_IN_4 10
#define DEVOLVE A0

//SAIDAS
#define MOEDA_OUT_100 2
#define MOEDA_OUT_50 1
#define MOEDA_OUT_25 0
#define PROD_OUT_1 9
#define PROD_OUT_2 8
#define PROD_OUT_3 7
#define PROD_OUT_4 6

LiquidCrystal_I2C lcd(0x27,16,2);  

void mens_imprime_string(String mensagem, int linha){
  if(linha==0)    
    lcd.clear();
  lcd.setCursor(0,linha);
  lcd.print(mensagem);  
}

void mens_imprime_saldo(){
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Saldo:"); 
  lcd.setCursor(0,1);
  lcd.print(float(saldoUsuario)/100);
}

void mens_abertura(){  
  lcd.setCursor(0,0);
  lcd.print("Prog. Sistemas"); 
  lcd.setCursor(0,1);
  lcd.print("Embarcados");
  delay(2000);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Anderson, Cesar");
  lcd.setCursor(0,1);
  lcd.print("Jonatan, Leandro");
  delay(2000);
}

void mens_insira_moedas(){
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Insira somente"); 
  lcd.setCursor(0,1);
  lcd.print("moedas de $1,00");
  //delay(2000);
  lcd.clear();
  lcd.setCursor(0,1);
  lcd.print("$0,50 ou $0,25,");
  //delay(2000);  
}

void setup(){
    
  pinMode(MOEDA_IN_100,INPUT);
  pinMode(MOEDA_IN_100,INPUT);
  pinMode(MOEDA_IN_50,INPUT);
  pinMode(PROD_IN_1,INPUT);
  pinMode(PROD_IN_2,INPUT);
  pinMode(PROD_IN_3,INPUT);
  pinMode(PROD_IN_4,INPUT);
  pinMode(DEVOLVE,INPUT);

  pinMode(MOEDA_OUT_100,OUTPUT);
  pinMode(MOEDA_OUT_50,OUTPUT);
  pinMode(MOEDA_OUT_25,OUTPUT);
  pinMode(PROD_OUT_1,OUTPUT);
  pinMode(PROD_OUT_2,OUTPUT);
  pinMode(PROD_OUT_3,OUTPUT);
  pinMode(PROD_OUT_4,OUTPUT);

  // CONFIGURAÇÃO DO LCD
  lcd.init();          
  lcd.backlight();
 
  //mens_abertura();
 
  estado = 1;//fim do setup passa para estado 1
}

void loop(){
    switch (estado){
        case 0:
            setup();
        break;
        case 1://state1 do diagrama 
            mens_imprime_string("Insira somente", 0);//função mens_insira_moedas() pronto
            mens_imprime_string("moedas de $1,00,", 1);
            getMillis=millis();
            do {
              if(getMillis+2000 < millis()){ // inverti o "<" de proposito pq o arduino bugou
                mens_imprime_string("$0,50 ou $0,25  ", 1);
              }
              if( digitalRead(PROD_IN_1) == 1 ||
                  digitalRead(PROD_IN_2) == 1 ||
                  digitalRead(PROD_IN_3) == 1 ||
                  digitalRead(PROD_IN_4) == 1 ||
                  digitalRead(DEVOLVE) ==1){
                  estado = 4;
             }
             if( digitalRead(MOEDA_IN_100) == 1 ||
                  digitalRead(MOEDA_IN_25) == 1 ||
                  digitalRead(MOEDA_IN_50) == 1){
                 estado = 2;
             }  
            }while((getMillis+4000) > millis() && estado == 1);// no meu pc o "||"" as vezes funciona como "&&"", nao sei porque
        break;
        case 2://state2 do diagrama
            if(digitalRead(MOEDA_IN_100)){
                while(digitalRead(MOEDA_IN_100)){
                    //espera ate soltar o botao
                }
                saldoUsuario = saldoUsuario+100;
                qtdMoeda100++;
            }
            else if(digitalRead(MOEDA_IN_50)){
                while(digitalRead(MOEDA_IN_50)){
                    //espera ate soltar o botao
                }
                saldoUsuario = saldoUsuario+50;
                qtdMoeda50++;
            }
            else{
                while(digitalRead(MOEDA_IN_25)){
                    //espera ate soltar o botao
                }
                saldoUsuario = saldoUsuario+25;
                qtdMoeda25++;
            }
            estado = 3;
        break;
        case 3:// Estado 3 (pronto - testar)
        //em desenvolvimento contador de tempo
            getMillis=millis();
            mens_imprime_saldo();//Exibe saldo do usuario
            while(estado==3)
            {
                if((millis()-getMillis) > 30000)//condição para ir para o estado 1 após 30 segundos
                {
                    estado=5;
                }
                else if( digitalRead(MOEDA_IN_100) == 1 ||//Entra no estado se o cliente estiver inserindo moedas
                    digitalRead(MOEDA_IN_25) == 1 ||
                    digitalRead(MOEDA_IN_50) == 1){
                    estado=2;
                    }
                else if( digitalRead(PROD_IN_1) == 1 ||//Entra no estado se o cliente comprou 1 produto
                         digitalRead(PROD_IN_2) == 1 ||
                         digitalRead(PROD_IN_3) == 1 ||
                         digitalRead(PROD_IN_4) == 1){
                    estado = 6;
                    }
                else if(digitalRead(DEVOLVE)==1){//Entra no estado caso haja troco.
                    estado=5;//SubmachineState1
                    }
            }
            break;
        case 4:// Estado 4 (pronto - testar)
            mens_imprime_string("Saldo", 0);         // exibe mensagem "Saldo " linha 0 do display
            mens_imprime_string("insuficiente!", 1); // exibe mensagem "insuficiente!" linha 1 do display
            delay(5000);
            estado = 1;
            break;
        case 5://substatemachine5 do diagrama (falta fazer função devolveSaldo e validar)
            if(devolveSaldoUsuario()){
                estado = 2;
            }
            else {
                estado = 1;
            }
        break;
        case 6://state6 do diagrama (pronto - falta testar)
            if( digitalRead(PROD_IN_1) == 1 && (qtdProduto1) == 0 ||
                digitalRead(PROD_IN_2) == 1 && (qtdProduto2) == 0 ||
                digitalRead(PROD_IN_3) == 1 && (qtdProduto3) == 0 ||
                digitalRead(PROD_IN_4) == 1 && (qtdProduto4) == 0)//condição para ir ao estado 7
                estado = 7;
            else if( digitalRead(PROD_IN_1) == 1 && (saldoUsuario) < 100 ||
                digitalRead(PROD_IN_2) == 1 && (saldoUsuario) < 150 ||
                digitalRead(PROD_IN_3) == 1 && (saldoUsuario) < 325 ||
                digitalRead(PROD_IN_4) == 1 && (saldoUsuario) < 500)//condição para ir ao estado 8
                estado = 8;    
            else if(digitalRead(PROD_IN_1) == 1 && (saldoUsuario) >= 100)//condição para ir ao estado 9
                estado = 9;
            else if(digitalRead(PROD_IN_2) == 1 && (saldoUsuario) >= 150)//condição para ir ao estado 10
                estado = 10;
            else if(digitalRead(PROD_IN_3) == 1 && (saldoUsuario) >= 320)//condição para ir ao estado 11
                estado = 11;
            else if(digitalRead(PROD_IN_4) == 1 && (saldoUsuario) >= 500)//condição para ir ao estado 12
                estado = 12;
        break;       
        case 7:// Estado 7 (pronto - testar) 
            mens_imprime_string("Produto", 0);         // exibe mensagem "Produto " linha 0 do display
            mens_imprime_string("indisponivel!", 1);  // exibe mensagem "indisponivel!" linha 1 do display
            delay(2000);
            estado = 3;
        break;
        case 8://state8 do diagrama (pronto - falta testar)
            mens_imprime_string("Saldo", 0);         // exibe mensagem "Saldo " linha 0 do display
            mens_imprime_string("insuficiente!", 1); // exibe mensagem "insuficiente!" linha 1 do display
            delay(2000);
            estado = 3;
        break;
        case 9:// Estado 9 (pronto - testar)
            saldoUsuario = saldoUsuario - 100;
            digitalWrite(PROD_OUT_1, HIGH);
            qtdProduto1--;
            mens_imprime_string("Produto R$1,00", 0);// exibe mensagem "Produto R$1,00 " linha 0 do display
            mens_imprime_string("foi liberado!", 1); // exibe mensagem "foi liberado!" linha 1 do display
            delay(2000);
            estado = 13;
        break;
        case 10:// Estado 10(pronto - testar)
            saldoUsuario = saldoUsuario - 150;
            digitalWrite(PROD_OUT_2, HIGH);
            qtdProduto2--;
            mens_imprime_string("Produto R$1,50", 0); // exibe mensagem "Produto R$1,50 " linha 0 do display
            mens_imprime_string("foi liberado!", 1); // exibe mensagem "foi liberado!" linha 1 do display
            delay(2000);
            estado = 13;
        break;
        case 11:// Estado 11 (pronto - testar)
            saldoUsuario = saldoUsuario - 325;
            digitalWrite(PROD_OUT_3, HIGH);
            qtdProduto3--;
            mens_imprime_string("Produto R$3,25", 0); // exibe mensagem "Produto R$3,25 " linha 0 do display
            mens_imprime_string("foi liberado!", 1); // exibe mensagem "foi liberado!" linha 1 do display
            delay(2000);
            estado = 13;
        break;
        case 12:// Estado 12 (pronto - testar)
            saldoUsuario = saldoUsuario - 500;
            digitalWrite(PROD_OUT_4, HIGH);
            qtdProduto4--;
            mens_imprime_string("Produto R$5,00", 0); // exibe mensagem "Produto R$5,00 " linha 0 do display
            mens_imprime_string("foi liberado!", 1); // exibe mensagem "foi liberado!" linha 1 do display
            delay(2000);
            estado = 13;
        break;
        case 13:// Estado 13 (pronto - testar)
            digitalWrite(PROD_OUT_1, LOW);
            digitalWrite(PROD_OUT_2, LOW);
            digitalWrite(PROD_OUT_3, LOW);
            digitalWrite(PROD_OUT_4, LOW);
            if(saldoUsuario == 0)
              estado = 1;
            else
              estado = 3;
        break;
        default:
            estado = 0;
        break;
    }
}


boolean devolveSaldoUsuario(){
  int estadoDevolve = 0;
  while(true){
    switch (estadoDevolve){
       case 0:
        digitalWrite(MOEDA_OUT_100, LOW);
        digitalWrite(MOEDA_OUT_50, LOW);
        digitalWrite(MOEDA_OUT_25, LOW);
        delay(500);
        if(saldoUsuario == 0){
          estadoDevolve = 5;
        }
        if(saldoUsuario > 0){
          estadoDevolve = 4;
        }
       break;
       case 1:
        if(saldoUsuario >= 100){
            estadoDevolve = 3;
        }
        if(saldoUsuario < 100){
            estadoDevolve = 2;
        }
       break;
       case 2:
        if(qtdMoeda50 > 0){
            estadoDevolve = 6;
        }
        else{
            estadoDevolve = 8;
        }
       break;
       case 3:
        digitalWrite(MOEDA_OUT_100, HIGH);
        saldoUsuario = saldoUsuario - 100;
        qtdMoeda100--;
        mens_imprime_saldo();//debug
        delay(1000);
        estadoDevolve = 0;
       break;
       case 4:
        if(qtdMoeda100 > 0){
            estadoDevolve = 1;
        }
        else{
            estadoDevolve = 2;
        }
       break;
       case 5:
        return 0;
       break;
       case 6:
        if(saldoUsuario >= 50){
            estadoDevolve = 7;
        }
        else{
            estadoDevolve = 8;
        }
       break;
       case 7:
        digitalWrite(MOEDA_OUT_50, HIGH);
        saldoUsuario = saldoUsuario - 50;
        qtdMoeda50--;
        mens_imprime_saldo();//debug
        delay(1000);
        estadoDevolve = 0;
       break;
       case 8:
        if(qtdMoeda25 > 0){
            estadoDevolve = 9;
        }
        else{
            estadoDevolve = 10;
        }
       break;
       case 9:
        if(saldoUsuario >= 25){
            estadoDevolve = 12;
        }
        else{
            estadoDevolve = 10;
        }
       break;
       case 10:
        mens_imprime_string("Impossivel",0);
        mens_imprime_string("devolver saldo",1);
        delay(1000);
        mens_imprime_string("deseja efetuar",0);
        mens_imprime_string("outra compra?",1);
        delay(1000);
        mens_imprime_string("Insira moeda",0);
        mens_imprime_string("para continuar",1);
        delay(1000);
        getMillis = millis();
        relogio = 0;
        while((getMillis+9000) > millis() && (estadoDevolve==10)){
            if( (getMillis+(relogio*1000)) < millis()){
             mens_imprime_string("Encerrando em:",0); 
             mens_imprime_string( String(9 - relogio),1);//imprime na segunda linha a contagem regressiva
             relogio++;
            }
            if( digitalRead(MOEDA_IN_100) == 1 ||//Entra no estado se o cliente estiver inserindo moedas
                digitalRead(MOEDA_IN_25) == 1 ||
                digitalRead(MOEDA_IN_50) == 1){
                    estadoDevolve=11;
            }
        }
        if(estadoDevolve==10){
            estadoDevolve=5;
        }
       break;
       case 11:
        return 1;
       break;
       case 12:
        digitalWrite(MOEDA_OUT_25, HIGH);
        saldoUsuario = saldoUsuario - 25;
        qtdMoeda25--;
        mens_imprime_saldo();//debug
        delay(1000);
        estadoDevolve = 0;
       break;
       default:
        estadoDevolve=10;
       break;
    }
  }
}

