# kursitestirov
void __fastcall TForm1::Button1Click(TObject *Sender)
{
        AnsiString A,B,C;        //переменные для считывания последовательности цифр с эдитов
        A=Edit1->Text;          //считывание
        B=Edit2->Text;
        C=Edit3->Text;
        bool imp=false;         //флаг невозможности решения
        if ( A.Length()<=C.Length() && B.Length()<=C.Length() && (( (A.Length()+1)>=C.Length() ) || ( (B.Length()+1)>=C.Length()) ))
        //проверка длины чисел при которых можно найти решение
        {
                while (A.Length()!=C.Length())      //для удобства сделаем числа одной длины, дополнив спереди нулями
                A="0"+A;
                while (B.Length()!=C.Length())
                B="0"+B;
                char a[100],b[100],c[100];          //чар массивы для чисел
                strcpy(a,A.c_str());
                strcpy(b,B.c_str());
                strcpy(c,C.c_str());
                char acopy[100], bcopy[100], ccopy[100];          //копии чисел(понадобится далее)
                strcpy(acopy,a), strcpy(bcopy,b),strcpy(ccopy,c);
                int n=C.Length(), x=0,y=0,z=0;              //x-a, y-b, z-c  - переменные, для проведения арифметики символов.
                bool flag=false;                          //флаг, показывающий был ли перенос в старший разряд
                for(int i=0; i<n; i++)                  //цикл по разбору чисел поэлементно, справа налево
                {
                        if ( a[n-1-i]=='?' ||  b[n-1-i]=='?' || c[n-1-i]=='?' )     //есть ли в N-ом порядке знак вопроса 
                        {
                                if ( a[n-1-i]=='?' &&  b[n-1-i]=='?' && c[n-1-i]=='?' )    //если на N-ом месте все три знака вопроса, будет множество решений, поэтому выбранно наиболее простое 
                                {
                                        a[n-1-i]='0';
                                        b[n-1-i]='0';
                                        if (flag) c[n-1-i]='1';                           //если был перенос, в С соответственно должно быть на 1н больше
                                        else c[n-1-i]='0';
                                        flag=false;
                                }
                                else                            //если не все три числа n-го порядка скрыты
                                {
                                        if(c[n-1-i]=='?')             
                                        {
                                                if(a[n-1-i]=='?')                   //если в С и А вопрос, то тоже имеем множество решений, опять же используем самое простое
                                                {
                                                        a[n-1-i]='0';
                                                        z=(int)(b[n-1-i]);
                                                        z=z-48;               //возникли трудности с переводом из символа чар в инт(т.к. функция atoi почему - то не сработала для одного элемента чар массива и за неимением других вариантов перевода, использовал функцию (int), которая дает код ascii символа, поэтому дальше будет встречаться -48 или +48 - это для перевода из кода в цифру.
                                                        if(flag)            //если был перенос
                                                        {
                                                                z++;
                                                                if (z==10)
                                                                {
                                                                        c[n-1-i]='0';
                                                                        flag=true;
                                                                }
                                                                else
                                                                {
                                                                        z=z+48;
                                                                        c[n-1-i]=(char)(z);
                                                                        flag=false;
                                                                }
                                                        }
                                                        else
                                                        {
                                                                z=z+48;
                                                                c[n-1-i]=(char)(z);
                                                                flag=false;
                                                        }
                                                }
                                                else                  //если вопрос в С и в В
                                                {
                                                        if(b[n-1-i]=='?')
                                                        {
                                                                b[n-1-i]='0';
                                                                z=(int)(a[n-1-i]);
                                                                z=z-48;
                                                                if(flag)
                                                                {
                                                                        z++;
                                                                        if (z==10)
                                                                        {
                                                                                c[n-1-i]='0';
                                                                                flag=true;
                                                                        }
                                                                        else
                                                                        {
                                                                                z=z+48;
                                                                                c[n-1-i]=(char)(z);
                                                                                flag=false;
                                                                        }
                                                                }
                                                                else
                                                                {
                                                                        z=z+48;
                                                                        c[n-1-i]=(char)(z);
                                                                        flag=false;
                                                                }
                                                        }
                                                        else            //если вопрос только в С
                                                        {
                                                                x=(int)(a[n-1-i]);
                                                                x=x-48;
                                                                y=(int)(b[n-1-i]);
                                                                y=y-48;
                                                                z=x+y;
                                                                if (flag) z++;
                                                                if(z>9)
                                                                {
                                                                        z=z-10;
                                                                        flag=true;
                                                                }
                                                                else flag=false;
                                                                z=z+48;
                                                                c[n-1-i]=(char)(z);
                                                        }
                                                }
                                        }
                                        else                  //в С число, а не вопрос
                                        {
                                                if(a[n-1-i]=='?' && b[n-1-i]!='?')      //если вопрос только в А 
                                                {
                                                        y=(int)(b[n-1-i]);
                                                        y=y-48;
                                                        z=(int)(c[n-1-i]);
                                                        z=z-48;
                                                        if (flag) z--;
                                                        if(y>z)
                                                        {
                                                                z=z+10;
                                                                flag=true;
                                                        }
                                                        else flag=false;
                                                        x=z-y;
                                                        x=x+48;
                                                        a[n-1-i]=(char)(x);
                                                }
                                                else
                                                {
                                                        if(b[n-1-i]=='?' && a[n-1-i]!='?')      //если вопрос только в В
                                                        {
                                                                x=(int)(a[n-1-i]);
                                                                x=x-48;
                                                                z=(int)(c[n-1-i]);
                                                                z=z-48;
                                                                if (flag) z--;
                                                                if(x>z)
                                                                {
                                                                        z=z+10;
                                                                        flag=true;
                                                                }
                                                                else flag=false;
                                                                y=z-x;
                                                                y=y+48;
                                                                b[n-1-i]=(char)(y);
                                                        }
                                                        else                //если вопрос в А и в В
                                                        {
                                                                z = (int) (c[n-1-i]);
                                                                z=z-48;
                                                                if (flag) z--;
                                                                if(z==-1)
                                                                {
                                                                        z=z+10;
                                                                        flag=true;
                                                                }
                                                                else flag=false;
                                                                a[n-1-i]='0';
                                                                z=z+48;
                                                                b[n-1-i]=(char)(z);
                                                        }
                                                }
                                        }
                                }
                        }
                        else              //если нет вопросов(тут будет отслеживаться флаг переноса, а так же будет проверка, возможно ли решение вовсе
                        {
                                x = (int) (a [n-1-i]);
                                x=x-48;
                                y = (int) (b [n-1-i]);
                                y=y-48;
                                z = (int) (c [n-1-i]);
                                z=z-48;
                                if (flag)
                                {
                                        z--;
                                        if (x+y==z+10 || x+y==z)
                                        {
                                                if (x+y==z) flag=false;
                                        }
                                        else imp=true;
                                }
                                else
                                {
                                        if (x+y==z+10 || x+y==z)
                                        {
                                                if (x+y==z+10) flag=true;
                                        }
                                        else
                                        {
                                                if(x+y+1==z || x+y+1==z+10)             //если по каким-то причинам в предыдущем порядке чисел нужен был перенос, но его не было
                                                {
                                                        if(x+y+1==z+10) flag=true;
                                                        if(acopy [n-i]=='?' && bcopy [n-i]=='?')        //для "создания переноса нам нужны предыдущие значения А и В, их сумма должна стать на 10 больше, что возможно только в случае, если и в А и в В были вопросики
                                                        {
                                                                x = (int) (a [n-i]);
                                                                x=x-48;
                                                                y = (int) (b [n-i]);
                                                                y=y-48;
                                                                int t=9-x;
                                                                a [n-i]='9';
                                                                y=y+(10-t);
                                                                if(y>9)
                                                                {
                                                                        imp=true;
                                                                        break;
                                                                }
                                                                y=y+48;
                                                                b [n-i]=(char)(y);
                                                        }
                                                        else imp=true;
                                                }
                                                else imp=true;
                                        }
                                }
                        }
                }
                while(1)                                            //затирание лишних нулей, дописанных в начале 
                {
                        if (A.Pos("0")==1) A=A.Delete(1,1);
                        else break;
                }
                while(1)
                {
                        if (B.Pos("0")==1) B=B.Delete(1,1);
                         else break;
                }
                A=AnsiString(a);
                B=AnsiString(b);
                C=AnsiString(c);
                Edit4->Text=A + "+" + B + "=" + C;          //вывод резульаьа


        }
        else Edit4->Text="impossible";          //если изначально длина чисел не подходила
        if (imp) Edit4->Text="impossible";        //если выявилась невозможность нахождения ответа в ходе решения
}
