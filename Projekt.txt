using System;
using System.Collections.Generic;
class RejestryProcesora 
{
       
  static Dictionary <string, string> ZainicjujRejestry() //inicjacja rejstrow czyli zawartosci rejestrow od AL do DH 
  {
    Dictionary<string, string> R = new Dictionary<string, string>(); //przydzielenie pamieci dla slownika
    R.Add("AL","0"); //nazwa rejestru i jego wartosc
    R.Add("AH","0");
    R.Add("BL","0");
    R.Add("BH","0");
    R.Add("CL","0");
    R.Add("CH","0");
    R.Add("DL","0");
    R.Add("DH","0");
    return R;
  }
   
  static void WypiszRejestry(Dictionary <string, string> Rej) //wypisanie nazw i zawartosci rejestrow
  {
    Console.WriteLine("Stan rejestrow procesora 8086:");  
    foreach(KeyValuePair<string, string> entry in Rej) //dla kazdego klucza i wartosci w slowniku
    {
         Console.WriteLine(entry.Key+" "+entry.Value); //wypisz klucz i slownik
    }
  } 
   
  static Dictionary <string, string> WczytajRejestry(Dictionary <string, string> Rej) //wczytanie rejestrow
  {
    string[] rej=new string[] {"AL", "AH", "BL", "BH", "CL", "CH", "DL", "DH"}; //lista nazw rejestrow
    Rej.Clear(); //czyszczenie slownika rejestrow z poprzedniego przebiegu petli
    for(int i=0;i<8;i++) //dla 8 pozycji tablicy rej (wyzej)
    {
         Console.Write("Podaj wartosc do zapisania w rejestrze "+rej[i]+":"); //wczytuje wartosc...
         int liczba=int.Parse(Console.ReadLine()); // ... do liczby calkowitej
         Rej.Add(rej[i], liczba.ToString("X"))  ; //i konwertuje na postac szesnastkowa
    }
    return Rej;
  } 
   
  static bool Poprawne(Dictionary <string, string> Rej)
  {
     string[] rej=new string[] {"AL", "AH", "BL", "BH", "CL", "CH", "DL", "DH"};
     bool odp=true;
     for(int i=0;i<8 && odp;i++)
     {
         int liczba=Convert.ToInt32(Rej[rej[i]], 16); //zamieniam liczy z szesnastowych na dziesietne
         if(liczba>255) odp=false; //jezeli wartosc przewyzsza 255, to jest zla wartosc
     }
     return odp;
  }
   
  static Dictionary <string, string> MOV(Dictionary <string, string> Rej, string a, string b)
  { //operacja mov, czyli skopiowanie z pozycji a do pozycji b
      Rej[a]=Rej[b];
      return Rej;
  }
   
  static Dictionary <string, string> XCHG(Dictionary <string, string> Rej, string a, string b)
  { //operacja zamiany, czyli zamieniam miejscami wartosci w pozycji a i b
      string temp; //string jako schowek
      temp=Rej[a]; //kopiuje do schowka zawartosc z a
      Rej[a]=Rej[b]; //potem do a kopiuje zawartosc z b
      Rej[b]=temp; //i do b ze schowka
      return Rej;
  } 
   
  static Dictionary <string, string> NOT(Dictionary <string, string> Rej, string a)
  {
      int temp; //tymczasowa liczba calkowita
      temp=Convert.ToInt32(Rej[a], 16); //z pozycji a wyjmuje liczbe szesnastkowa i zamieniam na dziesietna
      temp=255-temp; //odejmuje ja od 255, czyli otrzymuje zaprzeczenie bitow 0 na 1 i 1 na 0
      Rej[a]=temp.ToString("X"); //i z powrotem zamieniam na szestnastowa
      return Rej;
  } 
   
  static Dictionary <string, string> INC(Dictionary <string, string> Rej, string a)
  { //zwiekszenie liczby o 1
      int temp; //jak wyzej
      temp=Convert.ToInt32(Rej[a], 16);
      temp=temp+1; //po zamianie na liczbe dziesietna zwiekszam o 1
      Rej[a]=temp.ToString("X");
      return Rej;
  } 
   
  static Dictionary <string, string> DEC(Dictionary <string, string> Rej, string a)
  {
      int temp;
      temp=Convert.ToInt32(Rej[a], 16);
      temp=temp-1; 
      Rej[a]=temp.ToString("X");
      return Rej;
  }  
   
  static Dictionary <string, string> AND(Dictionary <string, string> Rej, string a, string b)
  {
      int temp1, temp2;
      temp1=Convert.ToInt32(Rej[a], 16);
      temp2=Convert.ToInt32(Rej[b], 16);
      temp1=temp1 & temp2;
      Rej[a]=temp1.ToString("X");
      return Rej;
  } 
   
  static Dictionary <string, string> OR(Dictionary <string, string> Rej, string a, string b)
  {
      int temp1, temp2;
      temp1=Convert.ToInt32(Rej[a], 16);
      temp2=Convert.ToInt32(Rej[b], 16);
      temp1=temp1 | temp2;
      Rej[a]=temp1.ToString("X");
      return Rej;
  } 
   
  static Dictionary <string, string> XOR(Dictionary <string, string> Rej, string a, string b)
  {
      int temp1, temp2;
      temp1=Convert.ToInt32(Rej[a], 16);
      temp2=Convert.ToInt32(Rej[b], 16);
      temp1=temp1 ^ temp2;
      Rej[a]=temp1.ToString("X");
      return Rej;
  } 
   
  static Dictionary <string, string> ADD(Dictionary <string, string> Rej, string a, string b)
  {
      int temp1, temp2;
      temp1=Convert.ToInt32(Rej[a], 16);
      temp2=Convert.ToInt32(Rej[b], 16);
      temp1=temp1 + temp2;
      Rej[a]=temp1.ToString("X");
      return Rej;
  } 
   
  static Dictionary <string, string> SUB(Dictionary <string, string> Rej, string a, string b)
  {
      int temp1, temp2;
      temp1=Convert.ToInt32(Rej[a], 16);
      temp2=Convert.ToInt32(Rej[b], 16);
      temp1=temp1 - temp2;
      Rej[a]=temp1.ToString("X");
      return Rej;
  }  
   
   
  static void Main() 
  {
    int wybor, instrukcja;  //wyor - czyli co zrobic w menu glownym. instrukcja - ktora instrukcja do wykonania
    string rej1, rej2; //rejestry do wczytania
    bool popr; //czy poprawnie wczytano wartosci rejestrow
    Dictionary<string, string> Rejestry= ZainicjujRejestry(); //wypelniam rejestry wartosciami zero
    do
    {
        WypiszRejestry(Rejestry); //wypisuje zawartosci rejestrow
        Console.WriteLine("Podaj numer operacji do wykonania:"); //menu widoczne dla usera
        Console.WriteLine("1 - zmiana zawartosci rejestrow");
        Console.WriteLine("2 - operacja miedzy rejestrami");
        Console.WriteLine("3 - wyjscie");
        Console.Write(">");
        wybor=int.Parse(Console.ReadLine()); //wczytuje numer operacji
        switch(wybor)
        {
            case 1: //jezeli bedzie zmiana rejestrow
              do
              {
                Rejestry=WczytajRejestry(Rejestry); //wczytuje je
                popr=Poprawne(Rejestry); //sprawdczam, czy poprawnie wczytano
                if (!popr) Console.WriteLine("Wczytanie nie jest 8 bitowe"); //jezeli nie, to pokazuje komunikat o bledzie
              }while(!popr);
            break;
            case 2: //beda dzialania na rejestrach
              Console.WriteLine("Podaj numer instrukcji do wykonania:"); //wybor instrukcji do wykonania
              Console.WriteLine("1 - MOV"); //lista instrukcji
              Console.WriteLine("2 - XCHG");
              Console.WriteLine("3 - NOT");
              Console.WriteLine("4 - INC");
              Console.WriteLine("5 - DEC");
              Console.WriteLine("6 - AND");
              Console.WriteLine("7 - OR");
              Console.WriteLine("8 - XOR");
              Console.WriteLine("9 - ADD");
              Console.WriteLine("10 - SUB");
              instrukcja=int.Parse(Console.ReadLine()); //wczytanie ktora instrukcja do wykonania
              switch(instrukcja) //sprawdzam numer instrukcji
              {
                  case 1:
                    Console.WriteLine("Dostepne rejestry: AL AH BL BH CL CH DL DH"); //wypisuje dostepne rejestry
                    Console.Write("Podaj zawartosc pierwszego rejestru dla instrukcji MOV:");
                    rej1=Console.ReadLine(); //wczytuje ktory rejestr do przetworzenia
                    rej1=rej1.ToUpper();
                    Console.Write("Podaj zawartosc drugiego rejestru dla instrukcji MOV:");
                    rej2=Console.ReadLine(); //ktory drugi rejestr do przetworzenia
                    rej2=rej2.ToUpper();
                    if (Rejestry.ContainsKey(rej1) && Rejestry.ContainsKey(rej2)) //jezeli rejestry sa poprawne, czyli ich nazwy
                    {
                      Rejestry=MOV(Rejestry, rej1, rej2); //to kopiuje do rej1 wartosc rej2
                    } else  Console.WriteLine("Bledny rejestr");
                  break;
                  case 2: //pozostale identyczne dzialanie
                    Console.WriteLine("Dostepne rejestry: AL AH BL BH CL CH DL DH");
                    Console.Write("Podaj zawartosc pierwszego rejestru dla instrukcji XCHG:");
                    rej1=Console.ReadLine();
                    rej1=rej1.ToUpper();
                    Console.Write("Podaj zawartosc drugiego rejestru dla instrukcji XCHG:");
                    rej2=Console.ReadLine();
                    rej2=rej2.ToUpper();
                    if (Rejestry.ContainsKey(rej1) && Rejestry.ContainsKey(rej2))
                    {
                      Rejestry=XCHG(Rejestry, rej1, rej2);
                    } else  Console.WriteLine("Bledny rejestr");
                  break;
                  case 3:
                    Console.WriteLine("Dostepne rejestry: AL AH BL BH CL CH DL DH");
                    Console.Write("Podaj zawartosc rejestru dla instrukcji NOT:");
                    rej1=Console.ReadLine();
                    rej1=rej1.ToUpper();
                    if (Rejestry.ContainsKey(rej1))
                    {
                      Rejestry=NOT(Rejestry, rej1);
                    } else  Console.WriteLine("Bledny rejestr");
                  break;
                  case 4:
                    Console.WriteLine("Dostepne rejestry: AL AH BL BH CL CH DL DH");
                    Console.Write("Podaj zawartosc rejestru dla instrukcji INC:");
                    rej1=Console.ReadLine();
                    rej1=rej1.ToUpper();
                    if (Rejestry.ContainsKey(rej1))
                    {
                      Rejestry=INC(Rejestry, rej1);
                    } else  Console.WriteLine("Bledny rejestr");
                  break;
                  case 5:
                    Console.WriteLine("Dostepne rejestry: AL AH BL BH CL CH DL DH");
                    Console.Write("Podaj zawartosc rejestru dla instrukcji DEC:");
                    rej1=Console.ReadLine();
                    rej1=rej1.ToUpper();
                    if (Rejestry.ContainsKey(rej1))
                    {
                      Rejestry=DEC(Rejestry, rej1);
                    } else  Console.WriteLine("Bledny rejestr");
                  break;
                  case 6:
                    Console.WriteLine("Dostepne rejestry: AL AH BL BH CL CH DL DH");
                    Console.Write("Podaj zawartosc pierwszego rejestru dla instrukcji AND:");
                    rej1=Console.ReadLine();
                    rej1=rej1.ToUpper();
                    Console.Write("Podaj zawartosc drugiego rejestru dla instrukcji AND:");
                    rej2=Console.ReadLine();
                    rej2=rej2.ToUpper();
                    if (Rejestry.ContainsKey(rej1) && Rejestry.ContainsKey(rej2))
                    {
                      Rejestry=AND(Rejestry, rej1, rej2);
                    } else  Console.WriteLine("Bledny rejestr");
                  break;
                  case 7:
                    Console.WriteLine("Dostepne rejestry: AL AH BL BH CL CH DL DH");
                    Console.Write("Podaj zawartosc pierwszego rejestru dla instrukcji OR:");
                    rej1=Console.ReadLine();
                    rej1=rej1.ToUpper();
                    Console.Write("Podaj zawartosc drugiego rejestru dla instrukcji OR:");
                    rej2=Console.ReadLine();
                    rej2=rej2.ToUpper();
                    if (Rejestry.ContainsKey(rej1) && Rejestry.ContainsKey(rej2))
                    {
                      Rejestry=OR(Rejestry, rej1, rej2);
                    } else  Console.WriteLine("Bledny rejestr");
                  break;
                  case 8:
                    Console.WriteLine("Dostepne rejestry: AL AH BL BH CL CH DL DH");
                    Console.Write("Podaj zawartosc pierwszego rejestru dla instrukcji XOR:");
                    rej1=Console.ReadLine();
                    rej1=rej1.ToUpper();
                    Console.Write("Podaj zawartosc drugiego rejestru dla instrukcji XOR:");
                    rej2=Console.ReadLine();
                    rej2=rej2.ToUpper();
                    if (Rejestry.ContainsKey(rej1) && Rejestry.ContainsKey(rej2))
                    {
                      Rejestry=XOR(Rejestry, rej1, rej2);
                    } else  Console.WriteLine("Bledny rejestr");
                  break;
                  case 9:
                    Console.WriteLine("Dostepne rejestry: AL AH BL BH CL CH DL DH");
                    Console.Write("Podaj zawartosc pierwszego rejestru dla instrukcji ADD:");
                    rej1=Console.ReadLine();
                    rej1=rej1.ToUpper();
                    Console.Write("Podaj zawartosc drugiego rejestru dla instrukcji ADD:");
                    rej2=Console.ReadLine();
                    rej2=rej2.ToUpper();
                    if (Rejestry.ContainsKey(rej1) && Rejestry.ContainsKey(rej2))
                    {
                      Rejestry=ADD(Rejestry, rej1, rej2);
                    } else  Console.WriteLine("Bledny rejestr");
                  break;
                  case 10:
                    Console.WriteLine("Dostepne rejestry: AL AH BL BH CL CH DL DH");
                    Console.Write("Podaj zawartosc pierwszego rejestru dla instrukcji SUB:");
                    rej1=Console.ReadLine();
                    rej1=rej1.ToUpper();
                    Console.Write("Podaj zawartosc drugiego rejestru dla instrukcji SUB:");
                    rej2=Console.ReadLine();
                    rej2=rej2.ToUpper();
                    if (Rejestry.ContainsKey(rej1) && Rejestry.ContainsKey(rej2))
                    {
                      Rejestry=SUB(Rejestry, rej1, rej2);
                    } else  Console.WriteLine("Bledny rejestr");
                  break;
                  default:
                    Console.WriteLine("Bledna instrukcja");
                  break;
              }
            break;
        }
    }while(wybor!=3);
  }
}

/*
    //int intValue = int.Parse(rej[i], System.Globalization.NumberStyles.HexNumber);
    int myInt = 2934;
     string myHex = myInt.ToString("X");  // Gives you hexadecimal
     Console.WriteLine(myHex);
     int myNewInt = Convert.ToInt32(myHex, 16);  // Back to int again.
            Console.WriteLine(myNewInt);*/