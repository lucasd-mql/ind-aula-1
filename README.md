#property copyright "Acesse https://lucasd-programadormql.com.br/"
#property link      "https://lucasd-programadormql.com.br/"
#property strict


//Propriedades essênciais do indicador
#property indicator_chart_window //Janela que o indicador vai ficar
#property indicator_buffers  2 //Número de buffers que o indicador vai ter
#property indicator_color1  clrBlue //Cor do buffer 1
#property indicator_color2  clrRed //Cor do buffer 2
#property indicator_width1  4 //Espessura do buffer 1
#property indicator_width2  4 //Espessura do buffer 2



//Parâmetros configuráveis do indicador
input  int    Barras    = 1440;



double Compra[],Venda[];//Declaramos os arrays, buffers de compra e venda


void OnDeinit(const int reason)
{
}
int OnInit()
{
  
    //Inicialização do indicador, é chamada apenas uma vez, quando o indicador é carregado no gráfico
    
    SetIndexBuffer(0,Compra);//Transformamos o array Compra em buffer
    SetIndexStyle(0,DRAW_ARROW);//Definimos o tipo de desenho
    SetIndexArrow(0,241);//Definimos o código da seta

    SetIndexBuffer(1,Venda);//Transformamos o array Venda em buffer
    SetIndexStyle(1,DRAW_ARROW);//Definimos o tipo de desenho
    SetIndexArrow(1,242);//Definimos o código da seta
   

   return(INIT_SUCCEEDED);


}
int OnCalculate(const int rates_total,
                const int prev_calculated,
                const datetime &time[],
                const double &open[],
                const double &high[],
                const double &low[],
                const double &close[],
                const long &tick_volume[],
                const long &volume[],
                const int &spread[])
{
    
    
    //Função dos cálculos do indicador, que é chamada a cada movimentação que houver no gráfico
    
    double tam_cnd_atual = 0,tam_cnd_anterior = 0,tam_pavil_acima = 0,tam_pavil_abaixo = 0;
    
    for(int i = 0; i <= Barras ; i++)
      {  
            
           tam_cnd_atual = MathAbs(close[i]-open[i]);
           tam_cnd_anterior = MathAbs(close[i+1]-open[i+1]);
           
           tam_pavil_acima = high[i]-MathMax(close[i],open[i]);//Precisamos do ponto mais alto do corpo, se a abertura for mais alta que o fechamento usamos a abertura, e vice versa
           tam_pavil_abaixo = MathMin(close[i],open[i])-low[i];//Precisamos do ponto mais baixo do corpo, se a abertura for mais baixa que o fechamento usamos a abertura, e vice versa
           
           if(tam_pavil_acima > tam_pavil_abaixo && tam_pavil_acima > tam_cnd_atual)
             {
                 
                 Venda[i] = close[i];
             
             }
           

           if(tam_pavil_abaixo > tam_pavil_acima && tam_pavil_abaixo > tam_cnd_atual)
             {
                 
                 Compra[i] = close[i];
             
             }
          
           
           
    
      }
   
          
   return(rates_total);
}
//==========================================================================================
