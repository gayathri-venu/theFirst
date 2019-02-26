#include<fstream.h>
#include<conio.h>
#include<stdio.h>
#include<string.h>
#include<iomanip.h>
fstream finout,finout1;
int x;
class screen
{ public:
  int screen_no;
  int gold_seats,silver_seats,normal_seats;
  char movie_name[20];
  screen(){}
  screen(int s, int g, int sl, int n, char mov[20]){
  screen_no=s;
  gold_seats=g;
  silver_seats=sl;
  normal_seats=n;
  strcpy(movie_name,mov);
  }
}a,b,c;

class tickets
{ public:
  int booking_no;
  int price;
  char seat_type[10];
  int no_of_seats;
  char movie_name[20];
  tickets(){
  booking_no =100;
  }
} t;
void show_movies()
{
  //print the values of movie_name of the struct screen variables
  cout<<"\nNow Showing......... "<<endl;
  cout<<"Screen "<<a.screen_no<<":";
  puts(a.movie_name);
  cout<<"Screen "<<b.screen_no<<":";
  puts(b.movie_name);
  cout<<"Screen "<<c.screen_no<<":";
  puts(c.movie_name);

}
void booking(screen&s)
{ //input seat_type,no_of_seat
  cout<<"\n\t\t~~~~~~~~~Screen\t"<<s.screen_no<<"~~~~~~~~~~~";
  cout<<"\nAvailable no. of"<<setw(25)<<"Price";
  cout<<"\nGold seats:"<<s.gold_seats<<setw(30)<<"Rs 130/-";
  cout<<"\nSilver seats:"<<s.silver_seats<<setw(30)<<"Rs 110/-";
  cout<<"\nNormal seats:"<<s.normal_seats<<setw(30)<<"Rs 90/-";
  cout<<"\nEnter Seat type(gold/silver/normal):";
  gets(t.seat_type);
  cout<<"\nEnter no. of seats:";
  cin>>t.no_of_seats;
  cout<<"\nPlease note your booking no. as "<<++t.booking_no;
		if(strcmpi(t.seat_type,"gold")==0)
  { if(t.no_of_seats<s.gold_seats)
	{ s.gold_seats-=t.no_of_seats;
	 t.price=t.no_of_seats*130;
	 }
	 else
	  {cout<<"\nSeats of required type not available!!!"; }
	}
	if(strcmpi(t.seat_type,"silver")==0)
  { if(t.no_of_seats<s.silver_seats)
	 {s.silver_seats-=t.no_of_seats;
	 t.price=t.no_of_seats*110;
	 }
	 else
	 {cout<<"\nSeats of required type not available!!!"; }

	}
	if(strcmpi(t.seat_type,"normal")==0)
  { if(t.no_of_seats<s.silver_seats)
	{
	 s.normal_seats-=t.no_of_seats;
	 t.price=t.no_of_seats*90;
	 }
	 else
	 {cout<<"\nSeats of required type not available!!!"; }


	}


 finout.write((char*)&t,sizeof(t));


}
void buy_tickets()
{
  //input the movie name
	cout<<"\n\nEnter name of the movie:";
	gets(t.movie_name);
	if(strcmpi(t.movie_name,"Bahubali 2-The conclusion")==0)
	  booking(a);
	if(strcmpi(t.movie_name,"The Great Father")==0)
	  booking(b);
	if(strcmpi(t.movie_name,"Take Off")==0)
	  booking(c);

}
void confirm()
{ //show seat_type,no_of_seats,price
 cout<<"\nBooking no.:"<<t.booking_no;
 cout<<"\nSeat type booked:";
 puts(t.seat_type);
 cout<<"\nNo.of seats booked:"<<t.no_of_seats;
 cout<<"\nTotal cost:"<<t.price;
 cout<<"\n\t\t~*~*~*~*~*~*~*~*~*~*~*~*~*~*~*~*~*~";


}
void cancel_tickets()
{
  cout<<"\nPlease enter the booking no. to be cancelled:";
  cin>>x;
  finout1.open("MOVIEBAK.DAT",ios::out|ios::binary);
  finout.open("MOVIE.DAT",ios::in|ios::binary);
  while(finout.read((char*)&t,sizeof(t)))
  { if(x!=t.booking_no)
		  finout1.write((char*)&t,sizeof(t));

	}
	finout1.close();
	finout.close();
	finout.open("MOVIE.DAT",ios::out|ios::binary);
  finout1.open("MOVIEBAK.DAT",ios::in|ios::binary);
  while(finout1.read((char*)&t,sizeof(t)))
  {
		  finout.write((char*)&t,sizeof(t));

	}
	finout.close();
	finout1.close();
	cout<<"\nYour tickets have been cancelled!!!";
}
void main()
{
  char ch;
  int f;
  //open file for storing tickets
  //enter intial values for struct screen variables s1, s2, s3
  a= screen(1,100,80,30,"Bahubali 2-The conclusion");
  b= screen(2,100,80,35,"The Great Father");
  c=screen(3,110,90,40,"Take Off");


  do
  {
  cout<<"\n\t\t**********     Menu     **********";
  cout<<"\n\t\t1.Show movies";
  cout<<"\n\t\t2.Book tickets";
  cout<<"\n\t\t3.Cancel tickets";
  cout<<"\n\t\t4.Confirm";
  cout<<"\n\t\t5-Exit";
  cout<<"\n\t\tEnter your choice:";
  cin>>ch;
  switch(ch)
  {
	 case '1' :  show_movies();
		break;
	 case '2' :  finout.open("MOVIE.DAT",ios::app|ios::binary);
					 buy_tickets();
					 finout.close();
					 break;
	 case '3' :  cancel_tickets();
					 break;
	 case '4' :  cout<<"\nEnter the booking no. to be confirmed:";
					 cin>>x;
					 f=0 ;
					 finout.open("MOVIE.DAT",ios::in|ios::binary);
						while(finout.read((char*)&t,sizeof(t)))
					 {	if(x==t.booking_no)
						  { f=1;
							 cout<<"\nBooking Successful!!!";
							 cout<<"\nPlease confirm....";
							 confirm();
							}
					 }
					 if(f==0)
						cout<<"\nBooking Cancelled!!!";
					 finout.close();
					 break;
	 case '5' :  cout<<"\nThank you!";//not exit0 since it qiuts the pgm
					 break;               //v hav finout .close to do
	 default  :  cout<<"Invalid Entry!";
  }

  }while(ch!='5');
getch();
}


