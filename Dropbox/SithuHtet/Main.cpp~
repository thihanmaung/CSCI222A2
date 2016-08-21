#include <iostream>		// cin, cout
#include <fstream>		// ifstream, ofstream
#include <sstream>		// stringstream
#include <string>
#include <stdio.h>
#include <iomanip>      // std::setw
#include <stdlib.h>     /* atoi */
#include "Printer.h"
#include <termios.h> 	//password masking
#include <unistd.h>	//password masking
#include <vector>
#include <algorithm>
#include <ctime>
#include <map>

using namespace std;

class LoginUsers
{
	public:	
	string loginID;			//ID to login
	string pw;			//password for login
	int cNum;			//credit card number
	int cPw;			//credit card security number
	string status;
	int attempt = 0;
};


class StockPile
{
	// to record stock information.
	public:
	int itemid;
	string itemname;
	string itemcat;
	string itemsubcat;
	string day;
	string month;
	string year;
	int amtpunit;
	int qty;
	string tradate;
	
	friend ostream& operator << (ostream& os, const StockPile&);
	friend bool operator == (const StockPile&, const StockPile&);
	friend int operator - (const StockPile&, const StockPile&);
};



ostream& operator<< (ostream& os, const StockPile& item){
	os << right << setw(10) << " " << left << setw(25) << "Item ID" << ":" <<item.itemid<<endl;
	os << right << setw(10) << " " << left << setw(25) << "Item Name" << ":" << item.itemname << endl;
	os << right << setw(10) << " " << left << setw(25) << "Item Category" << ":" << item.itemcat << endl;
	os << right << setw(10) << " " << left << setw(25) << "Item Sub-Category" << ":" << item.itemsubcat << endl;
	os << right << setw(10) << " " << left << setw(25) << "Price/Amount Per Unit" << ":" << item.amtpunit << endl;
	os << right << setw(10) << " " << left << setw(25) << "Quantity" << ":" << item.qty << endl;
	os << right << setw(10) << " " << left << setw(25) << "Transacted Date" << ":" << item.day << " " << item.month << " " << item.year << endl;
	
	
	return os;
}

enum REPORT_SUMMARY{
	DAILY = 0, WEEKLY, MONTHLY, YEARLY
};

void readUserDatabase();                // read employee info from userdatabase.txt
void readStockDatabase();                // read stock info from stockdatabase.txt
void writeUserDatabase();               // write employee info to userdatabase.txt

void updateUserAccounts();
bool checkloginOK(); 			// login function
void storeduserdata(string,int);	// store read data
void storedstockdata(string,int);	// store read data
string getpw(string);			// get corresponding password of ID user
int getname(string);			// get the name of user
void displayoption(int);		// get option by user
void displaystockpile();		// display stock
void addstock();			// add
void removestock();			// remove
void editstock();			// edit
void searchstockmain();
void searchstockcat();			// search by category
void searchstocksubcat();		// search by subcategory
void searchname();			// search by name
void sortstock();			// sorting
void ascdescorder();			// display ascending or descending order
void ascorder();			// sort by ascending
void descorder();			// sort by descending
//void dailyreport();			// daily report
//void weeklyreport();			// weekly report
//void monthlyreport();			// monthly report
//void yearlyreport();			// yearly report
void displaystockid();			// display all stock id
void displaystockinfobyid(int,int k =1);	// display stock by id
void displaystockinfobycat (int,int k=1);	// display stock by category
void displaystockinfobysubcat (int,int k=1);	// display stock by category
void displaystockinfobyname (int,int k=1);	// display stock by category
int getstockpilesize();			// return stock size
int getindex(string);			// get empty index
void createuser();			// create new log in user
int get_Integer();			// get integer input cin
int get_dateTime(string&, string&, string&);
void displaymainstockinfo(int k=1);	// display all stock
void displayamtorderinfo (int k=1);	//display amount/price info
void displayqtyorderinfo (int k=1);	//display quantity info
void sortamt();				// sort by ascending amount
void sortamtdesc();			// sort by descending amount
void sortprice();			// sort by price
void sortqtyasc();			// sort by ascending quantity
void sortqtydesc();			// sort by descending quantity
void thresholdalert();
void sortid();				// sort items by ID
void displaystockidnName();		// display stock item name

void display_daily_report_summary();
void display_weekly_report_summary();
void display_monthly_report_summary();
void display_yearly_report_summary();

void display_report(int);
bool sortbydate(const StockPile*, const StockPile*);
int datetimevalue(const string& day, const string& month, const string& year);

int month_value(string monthname);

//password masking			
int getch();				// get **** char
string getpass(const char* , bool );	// get passward
int stockpilesize();			// return occupied size	

//encrypt and decryption
string Encrypt(string&);		// encrypt string
string Decrypt(string strTarget);	// decrypt



const int MAX = 50;
vector<LoginUsers> users;	// Array without predefined size
vector<StockPile> stockpile;	// Array without predefined size
Printer printer;		// Printer class

static const string arr[] = {
	"January", "February", "March", "April", "May", "June", "July",
	"August", "September", "October", "November", "December"
	};
	
vector<string> monthnames (arr, arr+sizeof(arr)/sizeof(arr[0]));

int month_value(string monthname){
	int pos = find(monthnames.begin(), monthnames.end(), monthname) - monthnames.begin();
	return pos+1;
}

// Make a tm structure representing this date
std::tm make_tm(int year, int month, int day)
{
    std::tm tm = {0};
    tm.tm_year = year - 1900; // years count from 1900
    tm.tm_mon = month - 1;    // months count from January=0
    tm.tm_mday = day;         // days count from 1
    return tm;
}

int datetimevalue(const string& day, const string& month, const string& year){
	int iday = atoi(day.c_str());
	int imonth = month_value(month);
	int iyear = atoi(year.c_str());
	
	int datevalue = iday + imonth*100 + iyear*10000;
	
	return datevalue;
}

bool sortbydate(const StockPile* item0, const StockPile* item1){
	int date0 = datetimevalue(item0->day, item0->month, item0->year);
	int date1 = datetimevalue(item1->day, item1->month, item1->year);
	
	return date0 < date1;
}

bool operator<(const StockPile& s1, const StockPile& s2){
	int date1 = datetimevalue(s1.day, s1.month, s1.year);
	int date2 = datetimevalue(s2.day, s2.month, s2.year);
	
	return date1 < date2;
}

bool operator == (const StockPile& s1, const StockPile& s2){
	int date1 = datetimevalue(s1.day, s1.month, s1.year);
	int date2 = datetimevalue(s2.day, s2.month, s2.year);
	
	return date1 == date2;
}

int operator - (const StockPile& s1, const StockPile& s2){
	std::tm tm1 = make_tm(atoi(s1.year.c_str()),month_value(s1.month),atoi(s1.day.c_str()));    
	std::tm tm2 = make_tm(atoi(s2.year.c_str()),month_value(s2.month),atoi(s2.day.c_str())); 

	std::time_t time1 = std::mktime(&tm1);
	std::time_t time2 = std::mktime(&tm2);

	// Divide by the number of seconds in a day
	const int seconds_per_day = 60*60*24;
	std::time_t difference = (time1 - time2) / seconds_per_day;    
	int portable_difference = std::difftime(time1, time2) / seconds_per_day;

	return portable_difference;
}

void display_report(int report_type){
	
	vector<StockPile> stockpile_list;
	for(int i = 0; i <  stockpile.size(); ++i){
		StockPile item = stockpile[i];
		stockpile_list.push_back(item);
		
	}
	
	//sort(stockpile_list.begin(), stockpile_list.end(), sortbydate);
	sort(stockpile_list.begin(), stockpile_list.end());
	
	
	
	for(int i = 0; i != stockpile_list.size(); ++i){
		StockPile item0 = (stockpile_list[i]);
		//cout << item0 << endl;
		
		string date(item0.day + " " + item0.month + " " + item0.year);
		
		vector<StockPile> dataset;
		dataset.push_back(item0);
		
		switch(report_type){
			case DAILY: cout << "  " <<date << endl; break;
			case MONTHLY: cout << "  " << item0.month << " " << item0.year<< endl; break;
			case YEARLY: cout << "  " << item0.year<< endl; break;
			case WEEKLY: cout << "  " << date << endl; break;
			default: break;
		}
		
		cout << setw(20) << "StockItem";
		cout << setw(20) << "Item Name";
		cout << setw(20) << "Quantity";
		cout << setw(20) << "Amount(Per Unit)";
		cout << setw(20) << "Total Amount";
		cout << endl;
		
		for(int j = i+1; j != stockpile_list.size(); ++j){
			StockPile item1 = stockpile_list[j];
			
			if(report_type == DAILY){
				if(item0 == item1){
					dataset.push_back(item1);
					i = j;
				}
			}else if(report_type == MONTHLY){
				if(item0.year == item1.year){
					if(item0.month == item1.month){
						dataset.push_back(item1);
						i = j;
					}
				}
			}else if(report_type == YEARLY){
				if(item0.year == item1.year){
					dataset.push_back(item1);
					i = j;
				}
			}else if(report_type == WEEKLY){
				if(item1 - item0 <= 7){
					dataset.push_back(item1);
					i = j;
				}
			}
		}
		
		for(int j = 0; j != dataset.size(); ++j){
			StockPile listed = dataset[j];
			cout <<	setw(20) << listed.itemid;
			cout << setw(20) << listed.itemname;
			cout << setw(20) << listed.qty;
			cout << setw(20) << listed.amtpunit;
			cout << setw(20) << listed.amtpunit*listed.qty << endl;
		}
		cout << endl;
		dataset.clear();
	}
	
	
}

int main()
{

 printer.printmainheader();
 cout<<"	"<<endl;
 printer.printmenuheader();
 cout<<" 1) Log in"<<endl;
 cout<<" 2) Create user"<<endl;
 cout<<" 3) Search auction item"<<endl;

 int choice;
	cout<<endl<<"Select option :";
	cin>>choice;
	cout<<endl;	
	
 	if(choice==1)
	{
		system("clear");
		readUserDatabase(); //read file database
	//readStockDatabase();  //read file database
	printer.printmainheader(); //display
	
	printer.printloginheader();
	
	int tries = -1;//max 3 tries

	
	if(!checkloginOK()){
		return 0;
	}
	//thresholdalert();

	int option=-1; //initialise int
	do
	{
	printer.printdisplaymainmenu(); //display menu
	cout<<endl<<"        "<<"Enter Option :";	
	cin>>option;
	displayoption(option);
	}while(option!=0);
		
	}
	else if(choice==2)
	{
		readUserDatabase();
		createuser();
		main();
	}
	else if(choice==3)
	{
		readStockDatabase();
		searchstockmain();
		return -1;
	}
		
	
 
}

void displayoption(int option)
{
//option control functions
	switch(option)
	{
	case 1:
	displaystockpile();
	break;
	case 2:
	printer.printaddheader();
	addstock();
	break;
	case 3:
	removestock();
	break;
	case 4:
	editstock();
	break;
	case 5:
	searchstockmain();
	break;
	case 6:
	ascdescorder();
	break;
	case 7:
	printer.printdailyreport();		
	display_report(DAILY);
		break;
	
	case 8:
	printer.printweeklyreport();		
	display_report(WEEKLY);
		break;
	
	case 9:
	printer.printmonthlyreport();	
	display_report(MONTHLY); 
		break;
	
	case 10:
	printer.printyearlyreport();
	display_report(YEARLY);
		break;
	
	case 11:
	createuser();
	break;
	default:break;
	}
}

bool checkloginOK()
{
	string id,inputpw,storedpw,name;
	bool loginOK = false;
		
		bool correctUser = false;
		int idx;
		
		// check if user exists
		
		do{
			
			cin.clear();
			//cin.getline(cin, id);
			cout<< right << setw(10) << " " << "Enter login ID :";
			getline(cin, id);
			idx = getname(id);
			
			if(idx > -1){
				
				if(users[idx].status == "blocked"){
					cout << right << setw(10) << " " << "User is blocked" << endl;
				}else if(users[idx].status == "active"){
					correctUser = true;
				}
				else if(users[idx].status == "blacklist"){
					cout << right << setw(10) << " " << "Your accounted has been blacklisted. Contact with administrator for further details" << endl;
				}
			
			}
			else{
				cout << right << setw(10) << " " << "USER DOES NOT EXIST" << endl;
			}
			
		} 
		while(!correctUser); // keep prompting user id until correct user is hit
		
		
		LoginUsers user = users[idx]; // retrieve user attributes for password comparison
		
		
		// password input
		do{
			cin.clear();
			inputpw = getpass("Enter the password: ",true); // Show asterisks
			
			storedpw = getpw(id);
			
			// correct password
			if(Decrypt(inputpw)==storedpw){
				//name = getname(id);
				cout<< right << setw(10) << " " << "login success !"<<endl<<endl;
				cout<<"\E[1;32m	   WELCOME BACK "<< id <<"!!!\E[0m"<<endl;
				cout<<"";
				loginOK = true;
				break;
			}else{ // wrong password
				++user.attempt;
				cout<<"	Attempt number:"<<user.attempt<<endl;
				if(user.attempt >= 3){ // attempt reached maximum tries
					user.attempt = 3;
					user.status = "blocked"; // block the user
					cout << right << setw(10) << " " << "Login attempt exceeds maximum number " << endl;
					cout << right << setw(10) << " " <<" User " << id << " is now blocked. Please contact administrator to unblock the account." << endl;
					loginOK = false;

					// update userdatabase text file
					users[idx] = user; 
					updateUserAccounts();
					cout<<"";
					checkloginOK();
					
				}else{
					cout << right << setw(10) << " " << "INCORRECT PASSWORD" << endl;
				}
			}
			
		} while(user.status == "active"); // until user status is blocked
			
		return loginOK;
		
		
}

int getname(string id)
{
	for(int i=0;i<users.size(); i++) //check with stored user name and pw;
	{
		if(users[i].loginID==id)
		{
		//return users[i].loginID; //return userid
			//cout << "found" << endl;
			return i;
		}
	}
	return -1; //return this not found
}
string getpw(string id)
{
	for(int i=0;i<users.size(); i++) //check with stored user name and pw;
	{
		if(users[i].loginID==id)
		{
			return users[i].pw; //return userid password
		}
	}
	return "rubbishvalue"; //return this not found
}


void thresholdalert()
{
	for(int i=0;i<stockpile.size();i++)
{	
	if(stockpile[i].qty<=10)
		{
			cout<<"";
			cout<<stockpile[i].itemname<<"	\E[1;33m is lower than the minimum amount. Please restock the item	\E[0m"<<endl;
			cout<<"\E[1;33m 	 Minimum Amount: \E[0m" "10"<<endl;
			cout<<"\E[1;33m 	 Current Amount: \E[0m"<<stockpile[i].qty<<endl;
			cout<<""<<endl;

			
		}

}
}

void displaystockpile()
{
	printer.printinventorydisplayheader(); //display 
	
	sortid();
	
	for(int i = 0; i < stockpile.size(); ++i){
		cout << stockpile[i] << endl;
	}

	
	cout<<"   	Total "<<stockpile.size()<<" records"<<endl;
	cout<<"   	End of Stock Pile \n";
	
	

}

void displaymainstockinfo(int k)
{

	sortid();
	for(int i=0;i<stockpile.size(); i++) //check with stored user name and pw;
	{
		
		
		
		{
			
			
		
		if(k==1)
		{
			cout << stockpile[i] << endl;
		
		
		}
		else if(k==2)
		{
			cout << stockpile[i] << endl;
			
		}
		
		}

	}

	
	
}

void displayamtorderinfo(int k)
{

	for(int i=0;i<stockpile.size(); i++) //check with stored user name and pw;
	{
		
		{
		if(k==1)
		{
			cout << stockpile[i] << endl;
		
		}
		else if(k==2)
		{
			cout << stockpile[i] << endl;
		
		}
		

		
		}
	}
}

void displayqtyorderinfo(int k)
{
//for sorting
//printout selected id information, with highlighting
	for(int i=0;i<stockpile.size(); i++) //check with stored user name and pw;
	{
		
		{
		if(k==1)
		{
			cout << stockpile[i] << endl;
		
		}
		else if(k==2)
		{
			cout << stockpile[i] << endl;
		
		}
		

		
		}
	}
}

int stockpilesize()
{ 
	return stockpile.size();
}



void addstock()
{
	
	string itemid;
	string itemname;
	string itemcat;
	string itemsubcat;
	string day;
	string month;
	string year;
	int amtpunit;
	int qty;
	string tradate;
		
	
	
	
	cin.ignore(); 
	
	cout << "";
	cout<<"   	Item Name 		:";
	getline(cin, itemname);

	cout<<"	Item Category    	:";
	getline(cin, itemcat);

	cout<<"	Item Sub-Category	:";
	getline(cin, itemsubcat);
	
	cout<<"   	Price/Amount Per Unit   :";
	amtpunit = get_Integer();

	cout<<"	Quantity	 	:";
	qty = get_Integer();

	cout<<"	Transaction Date 	:"<<endl;
	cout<<"";
	get_dateTime(day, month, year);
	
	
	
	
	StockPile item;
	
	
	item.itemid = stockpile[stockpile.size()-1].itemid + 1;
	item.itemname = itemname;
	item.itemcat = itemcat;
	item.itemsubcat = itemsubcat;
	item.amtpunit = amtpunit;
	item.qty = qty;
	
	item.day = day;
	item.month = month;
	item.year = year;
	
	stockpile.push_back(item);
		
	writeUserDatabase(); //update stockdatabasefile
	cout<<"   	\E[1;29mItem ID "<<itemid<<" added...\E[0m"<<endl;
	sortid();
}

int get_dateTime(string& day, string& month, string& year)
{
	
	cout<<"\E[1;31m		Enter day   	:\E[0m ";
	cin>>day;
	cout<<endl;
	cout<<"\E[1;31m		Enter month 	:\E[0m ";
	cin>>month;
	cout<<endl;
	cout<<"\E[1;31m		Enter year  	:\E[0m ";
	cin>>year;
	cout<<endl;

return 0;


}

int get_Integer()		// Function that asks for Integer while ignoring invalid input such as char or string
{
	stringstream ss;                // Multi-purpose stringstream
        string tempString;		// Global string variable for various uses     
	
	int temp;		// Initialize a temporary Integer
	
	tempString = "";	// Initialize tempString back to null
	
	ss.clear();		// reset eof bit counter in stringstream
	ss.str("");		// clear the contents in stringstream

	getline(cin, tempString);

	

	ss.str(tempString);	// Put the contents of tempString into ss
	ss >> temp;		// Extract the Integer out of ss to temp

	return temp;		// Return the Integer value of user's choice

	


	
}

void removestock()
{
	printer.printdeleteheader();
	cout<<endl;
	
	displaystockidnName();
	string selectedid;
	
	cout<<"	Please choose 0 to go back"<<endl;
	cout<<"   	enter ID to remove :"; //getting input
	cin>>selectedid;
	cout<<endl;
	
	
	
	int i = getindex(selectedid);
	
	displaystockinfobyid(i); //display selected stock ID information
		
	if(i!=-1)
	{
	cout<<"   	are you sure you want to remove (Y/N) :"; //confirmation
	string yesno;
	cin>>yesno;

		if(yesno=="y" || yesno == "Y")
		{
		
		stockpile.erase(stockpile.begin() + i);
		cout<<"   	\E[1;29mItem ID "<<selectedid<<" deleted...\E[0m"<<endl;
		writeUserDatabase(); //update stockdatabasefile
		}

		
		else
		{
		cout<<"   	\E[1;31mDelete Fail...\E[0m"<<endl;
		}

	
	}
	
	
}

void editstock()
{
	

	printer.printeditheader();
	  
	displaystockidnName(); 
	string selectedid;
	cout<<"   	enter ID to edit :"; //getting input
	cin>>selectedid;
	cout<<"	Please choose 0 to go back"<<endl;
	//cout<<endl;
	

	int i = getindex(selectedid);
	if(i==-1)	
	return;
	displaystockinfobyid(i); //display selected stock ID information
	
	string itemid;
	string itemname;
	string itemcat;
	string itemsubcat;
	int amtpunit;
	int qty;
	string tradate;
	string day, month, year;
		
	
	
	
	cout<<"   	Item Name 		:";
	getline(cin, itemname);

	cout<<"	Item Category    	:";
	getline(cin, itemcat);

	cout<<"	Item Sub-Category	:";
	getline(cin, itemsubcat);
	
	cout<<"   	Price/Amount Per Unit   :";
	amtpunit = get_Integer();

	cout<<"	Quantity	 	:";
	qty = get_Integer();

	cout<<"	Transaction Date 	:"<<endl;
	cout<<"";
	get_dateTime(day, month, year);
	
	
	
	stockpile[i].itemname = itemname;
	stockpile[i].itemcat = itemcat;
	stockpile[i].itemsubcat = itemsubcat;
	stockpile[i].amtpunit = amtpunit;
	stockpile[i].qty = qty;
	stockpile[i].day = day;
	stockpile[i].month = month;
	stockpile[i].year = year;
	
		
	writeUserDatabase(); //update stockdatabasefile
	cout<<"   	\E[1;29mItem ID "<<selectedid<<" edited...\E[0m"<<endl;
}



void searchstockmain()
{
	printer.printsearchheader();
	cout<<"	1) Search Item by Name"<<endl;
	cout<<" 	2) Search Item by Category"<<endl;
	cout<<" 	3) Search Item by Subcategory"<<endl;
	cout<<" 	4) Go back to main menu"<<endl;
	int pick;
	cout<<endl<<"	Select option :";
	cin>>pick;
	cout<<endl;	
	
 	if(pick==1)
	{
		searchname();
		
	}
	else if(pick==2)
	{
		searchstockcat();
		
	}
	else if(pick==3)
	{
		searchstocksubcat();
	}
	else if(pick==4)
	{
		main();
	}	
	
		



}
void searchstockcat()
	{
	
	string selectedword;
	cout<<"   	enter category keyword to search :"; //getting input
	cin>>selectedword;
	cout<<endl;
	int found;
	
	int temp=0;

	for(int i = 0; i < stockpile.size(); i++)
     	{
     	if(stockpile[i].itemcat !="")
		{
		
		
			found = stockpile[i].itemcat.find(selectedword);
			
			if(found==0)
			
			{
				temp=1;
				sortid();
				displaystockinfobycat(i,1);
				
				
				

			}
		
		}
	
}

	

	if(temp==0)
	{
		cout<<"\E[1;31m		No such category in the item stock \E[0m"<<endl;
		return;
	}
	cout<<"	This is the category you are looking for"<<endl;
	searchstockmain();
	

}

void searchstocksubcat()
	{
	
	string chooseword;
	cout<<"   	enter category keyword to search :"; //getting input
	cin>>chooseword;
	cout<<endl;
	int found1;
	int temp1=0;

	for(int i = 0; i < stockpile.size(); i++)
     	{
     	if(stockpile[i].itemsubcat !="")
		{
		
		
			found1 = stockpile[i].itemsubcat.find(chooseword);
			
			if(found1==0)
			
			{
				temp1=1;
				sortid();				
				displaystockinfobysubcat(i,1);
				

			}
			
	
	
	
	
	
	}
	
}
	if(temp1==0)
	{
		cout<<"\E[1;31m		No such subcategory in the item stock \E[0m"<<endl;
		return;
	}
	cout<<" 	This is the item subcategory you are looking for"<<endl;
				searchstockmain();

}

void searchname()
	{
	
	string Keyword;
	cout<<"   	enter Item name to search :"; //getting input
	cin>>Keyword;
	cout<<endl;
	int found;
	
	int temp=0;

	for(int i = 0; i < stockpile.size(); i++)
     	{
     	if(stockpile[i].itemname !="")
		{
		
		
			found = stockpile[i].itemname.find(Keyword);
			
			if(found==0)
			
			{
				temp=1;
				sortid();
				displaystockinfobyname(i,1);
				
				
				

			}
		
		}
	
}

	

	if(temp==0)
	{
		cout<<"\E[1;31m		No such name in the posted items \E[0m"<<endl;
		return;
	}
	cout<<"	This is the item you are looking for"<<endl;
	searchstockmain();
	

}


void ascdescorder()
{
	printer.printascdescorder();

	cout<<"\E[1;35m	        1) sort by ascending order \E[0m"<<endl;
	cout<<"\E[1;35m	        2) sort by descending order \E[0m"<<endl;
	cout<<"		3) back to Main Menu"<<endl;

	int selection;
	cout<<endl<<"	Select option :";
	cin>>selection;
	cout<<endl;

	if(selection==1)
	{
		cout<<"\E[0;36m		1) Ascending Price \E[0m"<<endl;
		cout<<"\E[0;36m		2) Ascending Quantity \E[0m"<<endl;
		cout<<"\E[1;35m		3) Back to Sorting Menu \E[0m"<<endl;
		cout<<"		4)Back to Main Menu"<<endl;

		int choice;
		cout<<endl<<" Select your desired factor :";
		cin>>choice;
		cout<<endl;
		if(choice==1)
		{
			sortamt();
			displayamtorderinfo();
			
			cout<<"		Sorted by Price in Ascending order"<<endl;
			ascdescorder();

		}
		else if(choice==2)
		{
			sortqtyasc();
			displayqtyorderinfo();
	
			cout<<"		Sorted by Quantity in Ascending order"<<endl;
			ascdescorder();
		}
		else if(choice==3)
		{
			ascdescorder();
		}
		else if(choice==4)
		{
			return;
		}

	
	}
	else if(selection==2)
	{

		cout<<"\E[0;36m		1) Descending Price \E[0m"<<endl;
		cout<<"\E[0;36m		2) Descending Quantity \E[0m"<<endl;
		cout<<"\E[1;35m		3) Back to Sorting Menu \E[0m"<<endl;
		cout<<"		4)Back to Main Menu"<<endl;

		int nxtchoice;
		cout<<endl<<" Select your desired factor :";
		cin>>nxtchoice;
		cout<<endl;
		if(nxtchoice==1)
		{
			sortamtdesc();
			displayamtorderinfo();
			
			cout<<"		Sorted by Price in Ascending order"<<endl;
			ascdescorder();

		}
		else if(nxtchoice==2)
		{
			sortqtydesc();
			displayqtyorderinfo();
	
			cout<<"		Sorted by Quantity in Ascending order"<<endl;
			ascdescorder();
		}
		else if(nxtchoice==3)
		{
			ascdescorder();
		}
		else if(nxtchoice==4)
		{
			return;
		}

	
}
		else if(selection==3)
		{
			return;
		}
	

}

void sortid()
{
 	StockPile temp;
     for(int i = 0; i < stockpile.size(); i++)
     {
          for (int j = i + 1; j < stockpile.size(); j++)
          {
		  
		  {
			  if (stockpile[ i ].itemid > stockpile[ j ].itemid)  //comparing 
				   {
						 temp = stockpile[ i ];    //swapping entire struct
						 stockpile[ i ] = stockpile[ j ];
						 stockpile[ j ] = temp;
				   }
			  }
          }	

     }

}





void sortamt()
{
 	StockPile temp;
     for(int i = 0; i < stockpile.size(); i++)
     {
          for (int j = i + 1; j < stockpile.size(); j++)
          {
		 // if(stockpile[j].itemid !="")
		  {
		  if (stockpile[ i ].amtpunit > stockpile[ j ].amtpunit)  //comparing 
		       {
		             temp = stockpile[ i ];    //swapping entire struct
		             stockpile[ i ] = stockpile[ j ];
		             stockpile[ j ] = temp;
		       }
		  }
          }	

     }

}

void sortqtyasc()
{
 	StockPile temp;
     for(int i = 0; i < stockpile.size(); i++)
     {
          for (int j = i + 1; j < stockpile.size(); j++)
          {
		 
		  {
		  if (stockpile[ i ].qty > stockpile[ j ].qty)  //comparing 
		       {
		             temp = stockpile[ i ];    //swapping entire struct
		             stockpile[ i ] = stockpile[ j ];
		             stockpile[ j ] = temp;
		       }
		  }
          }	

     }

}


void sortamtdesc()
{
 	StockPile temp;
     for(int i = 0; i < stockpile.size(); i++)
     {
          for (int j = i + 1; j < stockpile.size(); j++)
          {
		  
		  {
		  if (stockpile[ j ].amtpunit > stockpile[ i ].amtpunit)  //comparing 
		       {
		             temp = stockpile[ j ];    //swapping entire struct
		             stockpile[ j ] = stockpile[ i ];
		             stockpile[ i ] = temp;
		       }
		  }
          }	

     }

}

void sortqtydesc()
{
 	StockPile temp;
     for(int i = 0; i < stockpile.size(); i++)
     {
          for (int j = i + 1; j < stockpile.size(); j++)
          {
		 
		  {
		  if (stockpile[ j ].qty > stockpile[ i ].qty)  //comparing 
		       {
		             temp = stockpile[ j ];    //swapping entire struct
		             stockpile[ j ] = stockpile[ i ];
		             stockpile[ i ] = temp;
		       }
		  }
          }	

     }

}






void displaystockid()
{
	for(int i=0;i<stockpile.size(); i++) //check with stored user name and pw;
	{
		
		{
		cout<<"   	Item ID         :\E[1;32m"<<stockpile[i].itemid<<"\E[0m"<<endl;
		}
	}
	cout<<endl;
}

void displaystockidnName()
{
	for(int i=0;i<stockpile.size(); i++) //check with stored user name and pw;
	{
		
		{
		cout<<"   	Item ID         :\E[1;32m"<<stockpile[i].itemid<<"\E[0m";
		cout<<"   	Item Name         :\E[1;32m"<<stockpile[i].itemname<<"\E[0m"<<endl;
		}
	}
	cout<<endl;
}

void displaystockinfobyid(int id,int k)
{

//printout selected id information, with highlighting
	int i=id;
	if(i==0)
	{
		return;
	}
	if(i>=1)
	{
	if(k==1)
	{
	cout<<"   	Item ID         	:"<<stockpile[i].itemid<<endl;
	cout<<"   	Item Name		:"<<stockpile[i].itemname<<endl;
	cout<<"	Item Category   	:"<<stockpile[i].itemcat<<endl;
	cout<<"	Item Sub-Category	:"<<stockpile[i].itemsubcat<<endl;       
	cout<<"   	Current bid price   :"<<stockpile[i].amtpunit<<endl;
	//cout<<"	Quantity         	:"<<stockpile[i].qty<<endl;
	//cout<<"	Transacted Date  	:"<<stockpile[i].tradate<<endl;
	cout<<"	Bidding end time  	:"<<stockpile[i].day << " " << stockpile[i].month << " " << stockpile[i].year<<endl;
	cout<<""<<endl;
	}
	else if(k==2)
	{
	cout<<"   	Item ID         	:"<<stockpile[i].itemid<<endl;
	cout<<"   	Item Name 		:"<<stockpile[i].itemname<<endl;
	cout<<"	Item Category    	:"<<stockpile[i].itemcat<<endl;
	cout<<"	Item Sub-Category	:"<<stockpile[i].itemsubcat<<endl;       
	cout<<"   	Current bid price   :"<<stockpile[i].amtpunit<<endl;
	//cout<<"	Quantity         	:"<<stockpile[i].qty<<endl;
	//cout<<"	Transacted Date  	:"<<stockpile[i].tradate<<endl;
	cout<<"	Bidding end time 	:"<<stockpile[i].day << " " << stockpile[i].month << " " << stockpile[i].year<<endl;
	cout<<""<<endl;
	}
	else
	{
	cout<<"   	\E[1;31mNo such Item ID...\E[0m"<<endl;
	}

}
	
	

	
	
}

void displaystockinfobyname(int id,int k)
{

//printout selected id information, with highlighting
	int i=id;
	if(i==0)
	{
		return;
	}
	if(i>=1)
	{
	if(k==1)
	{
	cout<<"   	Auction ID         	:"<<stockpile[i].itemid<<endl;
	cout<<"   	Item Name		:\E[1;35m"<<stockpile[i].itemname<<"\E[0m"<<endl;
	cout<<"	Item Category   	:"<<stockpile[i].itemcat<<endl;
	cout<<"	Item Sub-Category	:"<<stockpile[i].itemsubcat<<endl;       
	cout<<"   	Current bid price   :"<<stockpile[i].amtpunit<<endl;
	//cout<<"	Quantity         	:"<<stockpile[i].qty<<endl;
	//cout<<"	Transacted Date  	:"<<stockpile[i].tradate<<endl;
	cout<<"	Bidding end time  	:"<<stockpile[i].day << " " << stockpile[i].month << " " << stockpile[i].year<<endl;
	cout<<""<<endl;
	}
	else if(k==2)
	{
	cout<<"   	Auction ID         	:"<<stockpile[i].itemid<<endl;
	cout<<"   	Item Name 		:"<<stockpile[i].itemname<<endl;
	cout<<"	Item Category    	:"<<stockpile[i].itemcat<<endl;
	cout<<"	Item Sub-Category	:"<<stockpile[i].itemsubcat<<endl;       
	cout<<"   	Current bid price   :"<<stockpile[i].amtpunit<<endl;
	//cout<<"	Quantity         	:"<<stockpile[i].qty<<endl;
	//cout<<"	Transacted Date  	:"<<stockpile[i].tradate<<endl;
	cout<<"	Bidding end time  	:"<<stockpile[i].day << " " << stockpile[i].month << " " << stockpile[i].year<<endl;
	cout<<""<<endl;
	}
	else
	{
	cout<<"   	\E[1;31mNo such item name...\E[0m"<<endl;
	}

}
	
	

	
	
}


void displaystockinfobycat(int id,int k)
{

//printout selected id information, with highlighting
	int i=id;
	if(i!=-1)
	{
	if(k==1)
	{
	cout<<"   	Item ID         	:"<<stockpile[i].itemid<<endl;
	cout<<"   	Item Name        	:"<<stockpile[i].itemname<<endl;
	cout<<"        Item Category           :\E[1;35m"<<stockpile[i].itemcat<<"\E[0m"<<endl;
	cout<<"	Item Sub-Category	:"<<stockpile[i].itemsubcat<<endl;       
	cout<<"   	Current bid price   :"<<stockpile[i].amtpunit<<endl;
	//cout<<"	Quantity         	:"<<stockpile[i].qty<<endl;
	cout<<"	Bidding end time  	:"<<stockpile[i].tradate<<endl;
	cout<<""<<endl;
	}
	else if(k==2)
	{
	cout<<"   	Item ID         	:"<<stockpile[i].itemid<<endl;
	cout<<"    	Item Name        	:"<<stockpile[i].itemname<<endl;
	cout<<"	       Item Category           :\E[1;35m"<<stockpile[i].itemcat<<"\E[0m"<<endl;
	cout<<"	Item Sub-Category	:"<<stockpile[i].itemsubcat<<endl;       
	cout<<"   	Current bid price   :"<<stockpile[i].amtpunit<<endl;
	//cout<<"	Quantity         	:"<<stockpile[i].qty<<endl;
	cout<<"	Bidding end time  	:"<<stockpile[i].tradate<<endl;
	cout<<""<<endl;
	}
	
	}
	else
	{
	cout<<"   	\E[1;31mNo such Item Category...\E[0m"<<endl;
	}
}


void displaystockinfobysubcat(int id,int k)
{

//printout selected id information, with highlighting
	int i=id;
	if(i!=-1)
	{
	if(k==1)
	{
	cout<<"   	Item ID         	:"<<stockpile[i].itemid<<endl;
	cout<<"   	Item Name        	:"<<stockpile[i].itemname<<endl;
	cout<<"	Item Category           :"<<stockpile[i].itemcat<<endl;
	cout<<" 	Item Sub-Category	:\E[1;35m"<<stockpile[i].itemsubcat<<"\E[0m"<<endl;       
	cout<<"   	Current bid price   :"<<stockpile[i].amtpunit<<endl;
	//cout<<"	Quantity         	:"<<stockpile[i].qty<<endl;
	cout<<"	Bidding end time  	:"<<stockpile[i].tradate<<endl;
	cout<<""<<endl;
	}
	else if(k==2)
	{
	cout<<"   	Item ID         	:"<<stockpile[i].itemid<<endl;
	cout<<"   	Item Name        	:"<<stockpile[i].itemname<<endl;
	cout<<"	Item Category            :"<<stockpile[i].itemcat<<endl;
	cout<<"		Item Sub-Category	:\E[1;35m"<<stockpile[i].itemsubcat<<"\E[0m"<<endl;       
	cout<<"   	Current bid price   :"<<stockpile[i].amtpunit<<endl;
	cout<<"	Quantity         	:"<<stockpile[i].qty<<endl;
	cout<<"	Bidding end time  	:"<<stockpile[i].tradate<<endl;
	cout<<""<<endl;
	}
	
	}
	else
	{
	cout<<"   	\E[1;31mNo such Item Subcategory...\E[0m"<<endl;
	}
}


int getindex(string id)
{
	int i=-1;
	for(int i=0;i<stockpile.size(); i++) // get the index of array to be display out
	{
		if(stockpile[i].itemid==atoi(id.c_str()))
		{
		return i;
		}
	}
	return i;
}

int getstockpilesize()
{

	return stockpile.size();
}



void readStockDatabase()
{
	string tempString;
	
	ifstream stockDatabaseIN("stockdatabase.txt", ios::in); //initialise object
	stockpile.clear();
	if (stockDatabaseIN.is_open()) //test open
	{
		int i = 0;
		while (stockDatabaseIN.good())
		{
			getline (stockDatabaseIN, tempString); //reading line in file
			if (tempString == "")
				break;		
			
			storedstockdata(tempString,i);
			i++;
		} 
	}
	else
		cout << "Unable to open \"userdatabase.txt\"" << endl;


	stockDatabaseIN.close (); //close file
}

void readUserDatabase()
{
	string tempString;
	
	ifstream userDatabaseIN("userdatabase.txt", ios::in); //initialise object, reading file
	if (userDatabaseIN.is_open()) //test open
	{
		int i = 0;
		while (userDatabaseIN.good())
		{
			getline (userDatabaseIN, tempString); //reading line in file
			if (tempString == "")
				break;		
			storeduserdata(tempString,i);
			i++;
		} // end while
	} // end if
	else
		cout << "Unable to open \"userdatabase.txt\"" << endl;


	userDatabaseIN.close (); //close file
}

void writeUserDatabase()
{
	ofstream outfile;
	outfile.open ("stockdatabase.txt");
	
	
	if (!outfile)
	{
		cout << "\E[1;31mFile opened for writing failed\E[0m" << endl;
	}
	
	for(int i=0;i<stockpile.size(); i++) // get the index of array to be display out
	{
		//if(stockpile[i].itemid!="")
		{
		outfile<<stockpile[i].itemid<<":";
		outfile<<stockpile[i].itemname<<":";
		outfile<<stockpile[i].itemcat<<":";
		outfile<<stockpile[i].itemsubcat<<":";
		outfile<<stockpile[i].amtpunit<<":";
		outfile<<stockpile[i].qty<<":";
		//outfile<<stockpile[i].tradate<<endl;;
		outfile << stockpile[i].day << " " << stockpile[i].month << " " << stockpile[i].year << endl;
 		
		}
	}
	outfile.close();
	
}

void updateUserAccounts(){
	ofstream outfile;
	outfile.open ("userdatabase.txt");
	
	
	if (!outfile)
	{
		cout << "\E[1;31mFile opened for writing failed\E[0m" << endl;
	}
	
	for(int i=0;i<users.size(); i++) // get the index of array to be display out
	{
		//if(stockpile[i].itemid!="")
		{
		outfile<<users[i].loginID<<";";
		outfile<<users[i].pw<<";";
		outfile<<users[i].cNum<<";";
		outfile<<users[i].cPw<<";";
		outfile<<users[i].status<<endl;
		
		}
	}
	outfile.close();
}

void storedstockdata(string tempString,int i)
{
	string str = tempString;
	string word;
	stringstream stream(str);
	int col = 0;
	StockPile item;
	stockpile.push_back(item);
	while( getline(stream, word, ':') ) //split string
	{
		//storing into the right structure element
		
		if(col==0)
		{
			
		stockpile[i].itemid = atoi(word.c_str());
		}
		else if(col==1)
		{
		stockpile[i].itemname = word;
		}
		else if(col==2)
		{
		stockpile[i].itemcat = word;
		}
		else if(col==3)
		{
		stockpile[i].itemsubcat = word;
		}
		else if(col==4)
		{
		stockpile[i].amtpunit = atoi(word.c_str());
		}
		else if(col==5)
		{
		stockpile[i].qty = atoi(word.c_str());
		}
		else if(col==6)
		{
		
			stockpile[i].tradate = word;
			string temp;
			vector<string> tokens;
			stringstream stream1(word);
			while(getline(stream1, temp, ' ')){
				tokens.push_back(temp);
			}
			stockpile[i].day = tokens[0];
			stockpile[i].month = tokens[1];
			stockpile[i].year = tokens[2];
		}
		col = col +1; //next column data
	}
}


void storeduserdata(string tempString,int i)
{
	string str = tempString;
	string word;
	//int number;
	stringstream stream(str);
	int col = 0;
	LoginUsers user;
	while( getline(stream, word, ';') ) //split string
	{
		//storing into the right structure element
		if(col==0)
		{
		user.loginID = word;
		//cout << user.loginID << endl;
		}
		else if(col==1)
		{
			user.pw = word;
		}
		else if(col==2)
		{
			user.cNum = atoi(word.c_str());
		}
		else if(col==3)
		{
			user.cPw = atoi(word.c_str());
		}
		else if(col==4){
			user.status = word;
		}	
		col = col +1; //next column data
	}
	users.push_back(user);
}


int getch() {
    int ch;
    struct termios t_old, t_new;

    tcgetattr(STDIN_FILENO, &t_old);
    t_new = t_old;
    t_new.c_lflag &= ~(ICANON | ECHO);
    tcsetattr(STDIN_FILENO, TCSANOW, &t_new);

    ch = getchar();

    tcsetattr(STDIN_FILENO, TCSANOW, &t_old);
    return ch;
}


string getpass(const char *prompt, bool show_asterisk=true)
{
  const char BACKSPACE=127;
  const char RETURN=10;

  string password;
  unsigned char ch=0;

  cout << right << setw(10) << " " << prompt;

  while((ch=getch())!=RETURN)
    {
       if(ch==BACKSPACE)
         {
            if(password.length()!=0)
              {
                 if(show_asterisk)
                 cout <<"\b \b";
                 password.resize(password.length()-1);
              }
         }
       else
         {
             password+=ch;
			 cout << '*';
            
         }
    }
  cout <<endl;
  return password;
}

//password encryption method
string Encrypt(string &strTarget)
{
	int len = strTarget.length();
	char a;
	string strFinal(strTarget);

	for (int i = 0; i <= (len-1); i++)
	{
		a = strTarget.at(i); 
		int b = (int)a; //get the ASCII value of 'a'
		b += 2; //Mulitply the ASCII value by 2
		if (b > 254) { b = 254; }
		a = (char)b; //Set the new ASCII value back into the char
		strFinal.insert(i , 1, a); //Insert the new Character back into the string
	}
	string strEncrypted(strFinal, 0, len);
	strTarget = strEncrypted;
	
	return strTarget;
}

string Decrypt(string strTarget)
{
	int len = strTarget.length();
	char a;
	string strFinal(strTarget);

	for (int i = 0; i <= (len-1); i++)
	{
		a = strTarget.at(i);
		int b = (int)a;
		b -= 2;

		a = (char)b;
		strFinal.insert(i, 1, a);
	}
	string strDecrypted(strFinal, 0, len);
	return strDecrypted;
}

void createuser()
{
	printer.printadduserheader(); //display 
	
	string loginID1;
	string pw1;	
	int cardnum;
	int cardpw;		
		
	cout<<"Enter login ID:";
	cin>>loginID1;
	cout<<"Enter password:";
	cin>>pw1;
	cout<<"Enter credit card number:";
	cin>>cardnum;
	cout<<"Enter credit card security code number:";
	cin>>cardpw;	
	
	
	int size=0;
	for(int i=0;i<MAX-1; i++) //check with stored user name and pw;
	{
		
		{
		size = i;
		break;
		}
	}
	
	users[size].loginID = loginID1;
	users[size].pw = pw1;
	users[size].cNum = cardnum;
	users[size].cPw = cardpw;
	
	
	ofstream myfile;
	myfile.open ("userdatabase.txt", ios::out | ios::app ); //append to file
	
	myfile<<loginID1<<";"<<Decrypt(pw1)<<";"<<cardnum<<";"<<cardpw<<";"<<"active"<<endl;
	myfile.close();
	
	cout<<"\E[1;32mnew user "<<loginID1<<" added !!!\E[0m"<<endl;
}

