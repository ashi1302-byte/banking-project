# banking-project
//Project of banking by me.This project provides all banking systems.You can create a account,deposit money,withdraw monay and many more.
//banking project
//********************************************************************************************************************************

//                       HEADER FILE USED IN PROJECT

//*********************************************************************************************************************************

#include<iostream>
#include<fstream>
#include<cctype>
#include<iomanip>
using namespace std;

//********************************************************************************************************************************

//                       CLASS USED IN PROJECT

//*********************************************************************************************************************************

class account{
    public:
int acno;
char name[50];
int deposit;
char type;
void create_account(); //function to get data from user
void show_account(); //function to show data on screen
void modify(); //function to modify the data
void dep(int); //function to accept data and add amount
void draw(int); //function to withdraw data and add amount
void report() const; //function to show data in tabular form
int retacno() const; //function to return account number
int retdeposit() const;//function to return balance
char rettype() const;//function to return type of account
};
//class ends here
void account::create_account(){
cout<<"\n Enter the account no.: ";
cin>>acno;
cout<<"\n Enter the name of the account holder : ";
cin.ignore();
cin.getline(name,50);
cout<<"\n Enter the type of the account(C/S) ";
cin>>type;
type=toupper(type);
cout<<"\n The initial amount(>=500 for saving and for >=1000 for current)";
cin>>deposit;
cout<<"\n\n\n ACCOUNT CREATED ";
}
void account::show_account(){
cout<<"\n Account number : "<<acno;
cout<<"\n Account holder name : ";
cout<<name;
cout<<"\n Type of Account : "<<type;
cout<<"\n Balance account : "<<deposit;
}
void account::modify(){
cout<<"\n Account no. : "<<acno;
cout<<"\nModify Account holder name : ";
cin.ignore();
cin.getline(name,50);
cout<<"\n Modify Account type : ";
cin>>type;
type=toupper(type);
cout<<"Modify Balance Amount : ";
cin>>deposit;
}
void account::dep(int x){
deposit+=x;
}
void account::draw(int x){
deposit-=x;
}
void account::report()const{
cout<<acno<<setw(10)<<" "<<name<<setw(10)<<" "<<type<<setw(6)<<deposit<<endl;
}
int account::retacno()const{
return acno;
}
int account::retdeposit()const{
return deposit;
}
char account::rettype()const{
return type;
}
//**********************************************************************************************************************************
//     function declaration
//**********************************************************************************************************************************
void write_account(); //function to write account in binary file
void display_sp(int); //function to display details given by user
void modify_account(int); //function to modify record of file
void delete_account(int); //function to delete record of file
void display_all(); //function to display all account details
void deposit_withdraw(int,int); //function to deposit/withdraw amount for given account
void intro(); //introductary screen function

//**********************************************************************************************************************************
//           THE MAIN FUNCTION OF PROGRAM
//**********************************************************************************************************************************


int main(){
char ch;
int num;
intro();
do{
system("cls");
cout<<"\n\n\n MAIN MENU";
cout<<"\n\n\t01.NEW ACCOUNT";
cout<<"\n\n\t02.DEPOSIT AMOUNT";
cout<<"\n\n\t03.WITHDRAW AMOUNT";
cout<<"\n\n\t04.BALANCE ENQUIRY";
cout<<"\n\n\t05.ALL ACCOUNT HOLDER LIST";
cout<<"\n\n\t06.CLOSE AN ACCOUNT";
cout<<"\n\n\t07.MODIFY AN ACCOUNT";
cout<<"\n\n\t08.EXIT";
cin>>ch;
system("cls");
switch(ch){
case '1':
write_account();
break;
case '2':
cout<<"\n\n\tEnter the account no. : ";cin>>num;
deposit_withdraw(num,1);
break;
case '3':
cout<<"\n\n\tEnter the account no. : ";cin>>num;
deposit_withdraw(num,2);
break;
case '4':
cout<<"\n\n\tEnter the account no. : ";cin>>num;
display_sp(num);
break;
case '5':
display_all();
break;
break;
case '6':
cout<<"\n\n\tEnter the account no. : ";cin>>num;
delete_account(num);
break;
case '7':
cout<<"\n\n\tEnter the account no. : ";cin>>num;
modify_account(num);
break;
case '8':
cout<<"\n\n\tTHANKS FOR USING BANK MANAGEMENT SYSTEM ";
break;
default:cout<<"\a";
}
cin.ignore();
cin.get();
}while(ch!=8);
return 0;
}

//**********************************************************************************************************************************
//     function to write in file
//**********************************************************************************************************************************
void write_account(){
account ac;
ofstream outFile;
outFile.open("account.dat",ios::binary|ios::app);
ac.create_account();
outFile.write(reinterpret_cast<char*>(&ac),sizeof(account));
outFile.close();
}

//**********************************************************************************************************************************
//     function to read specific from file
//**********************************************************************************************************************************
void display_sp(int n){
account ac;
bool flag=false;
ifstream inFile;
inFile.open("account.dat",ios::binary);
if(!inFile){
cout<<"File could not open!!press any key...";
}
cout<<"\n BALANCE DETAILS \n";
while(inFile.read(reinterpret_cast<char*>(&ac),sizeof(account))){
if(ac.retacno()==n){
ac.show_account();
flag=true;
}
}
inFile.close();
if(flag==false)
cout<<"\n Account number does not exist";
}


//**********************************************************************************************************************************
//     function to modify record of file
//**********************************************************************************************************************************
void modify_account(int n){
bool found=false;
account ac;
fstream File;
File.open("account.dat",ios::binary|ios::in|ios::out);
if(!File)
{
cout<<"\n file could not be open!press any key...";
return;
}
while(!File.eof()&&found==false){
File.read(reinterpret_cast<char*>(&ac),sizeof(account));
if(ac.retacno()==n){
ac.show_account();
cout<<"\n\n Enter the new details of account: "<<endl;
ac.modify();
int pos=(-1)*static_cast<int>(sizeof(account));
File.seekp(pos,ios::cur);
File.write(reinterpret_cast<char*>(&ac),sizeof(account));
cout<<"\n\n\t Record updated";
found=true;
}
}
File.close();
if(found==false)
cout<<"\n\nRecord not found";
}
//**********************************************************************************************************************************
//     function to delete record of file
//**********************************************************************************************************************************
void delete_account(int n){
account ac;
ifstream inFile;
ofstream outFile;
inFile.open("account.dat",ios::binary);
if(!inFile){
cout<<"\n file could not be open!!press any key...";
return;
}
outFile.open("Temp.dat",ios::binary);
inFile.seekg(0,ios::beg);
while(inFile.read(reinterpret_cast<char*>(&ac),sizeof(account)))
{
if(ac.retacno()!=n){
outFile.write(reinterpret_cast<char*>(&ac),sizeof(account));
}
}
inFile.close();
outFile.close();
remove("account.dat");
remove("Temp.dat""account.dat");
cout<<"\n\n\t RECORD dELETED..";
}
//***************************************************************************************************************
//FUNCTION TO DISPLAY ALL ACCOUNTS DEPOSIT LISI
//****************************************************************************************************************
void display_all(){
account ac;
ifstream inFile;
inFile.open("account.dat",ios::binary);
if(!inFile){
cout<<"\n file could not be open!press any key...";
return;
}
cout<<"\n\n\tACCOUNT HOLDER LIST\n\n";
cout<<"==============================================================================================\n";
cout<<"A\c no.    NAME     Type Balance\n";
cout<<"===================================================================================================\n";
while(inFile.read(reinterpret_cast<char*>(&ac),sizeof(account)))
{
ac.report();
}
inFile.close();
}
//************************************************************************************************
// FUNCTION TO DEPOSIT AND WITHDRAW AMOUNTS
//************************************************************************************************
void deposit_withdraw(int n,int option){
int amt;
bool found=false;
account ac;
fstream File;
File.open("account.dat",ios::binary|ios::in|ios::out);
if(!File){
cout<<"\n file could not be open!press any key...";
return;
}
while(!File.eof()&&found==false){
File.read(reinterpret_cast<char*>(&ac),sizeof(account));
if(ac.retacno()==n){
ac.show_account();
if(option==1){
cout<<"\n\n\tTO DEPOSITE AMOUNT";
cout<<"\n\n\t Enter the amount to be deposited : ";
cin>>amt;
ac.dep(amt);
}
if(option==2){
cout<<"\n\n\tTO WITHDRAW AMOUNT";
cout<<"\n\n\t Enter the amount to be withdraw : ";
cin>>amt;
int bal=ac.retdeposit()-amt;
if((bal<500&&ac.rettype()=='S')||(bal<1000&&ac.rettype()=='C'))
cout<<"Insufficient balance";
else
ac.draw(amt);
}
int pos=(-1)*static_cast<int>(sizeof(ac));
File.seekp(pos,ios::cur);
File.write(reinterpret_cast<char*>(&ac),sizeof(account));
cout<<"\n\n\t Record updated";
found=true;
}
}
File.close();
if(found==false)
cout<<"\n\n Record not found";
}

//*********************************************************************************************************************
//                INTRODUCTION FUNCTION
//****************************************************************************************************************
void intro(){
cout<<"\n\n\t BANK";
cout<<"\n\n\tMANAGEMENNT";
cout<<"\n\n\tSYSTEM";
cin.get();
}
//*********************************************************************************************************************
//                END OF PROJECT
//****************************************************************************************************************
