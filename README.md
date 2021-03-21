# GroceryShop
#include<stdio.h>                   //contains printf,scanf etc
#include<conio.h>                   //contains delay(),getch(),gotoxy(),etc.
#include <stdlib.h>
#include<string.h>

//list of function prototype
char catagories[][15]={"Flour","Oil","Rice","Pulses","Soap","Shampoo"};
void returnfunc(void);
void mainmenu(void);
void additem(void);
void deleteitem(void);
void edititem(void);
void searchitem(void);


int  getdata();
int  checkid(int);
int t(void);

//list of global files that can be acceed form anywhere in program
FILE *fp,*ft,*fs;

//list of global variable
int s;
char finditem;
struct item
{
    int id;
    char stname[20];
    char itemname[20];
    int quantity;
    float Price;
    int count;
    char *cat;
};
struct item a;
int main()

{
      mainmenu();
   return 0;

}
void mainmenu()
{
    int i;
	printf("\t\t\t\t\tMAIN MENU\n");
	printf("\t\t\t\t\t1.Add Item   \n");
	printf("\t\t\t\t\t2.Delete Item\n");
	printf("\t\t\t\t\t3.Search Item\n");
	printf("\t\t\t\t\t4.Edit Item's Record\n");
	printf("\t\t\t\t\t5.Close Application\n");
	printf("\t\t\t\t\t Enter your choice:");
	switch(getch())
	{
		case '1':
		additem();
		break;
		case '2':
		deleteitem();
		break;
		case '3':
		searchitem();
	    break;
	    case '4':
		edititem();
		break;
	    case '5':
	    {
		exit(0);
	    }
	    default:
		{
		printf("\aWrong Entry!!Please re-entered correct option");
		if(getch())
		mainmenu();
		}

    }
}
void additem(void)    //funtion that add books
{
	int i;
	printf("\t\t\t\t\tSELECT CATEGOIES\n");
	printf("\t\t\t\t\t 1.Flour\n");
	printf("\t\t\t\t\t 2.Oil\n");
	printf("\t\t\t\t\t 3.Rice\n");
	printf("\t\t\t\t\t 4.Pulses\n");
	printf("\t\t\t\t\t 5.Soap\n");
	printf("\t\t\t\t\t 6.Shampoo\n");
	printf("\t\t\t\t\t 7.Back to main menu\n");
	printf("\t\t\t\t\t\n");
	printf("\t\t\t\t\t Enter your choice:");
	scanf("%d",&s);
	if(s==7)
	mainmenu() ;
	
	fp=fopen("abc.txt","ab+");
	if(getdata()==1)
	{
	a.cat=catagories[s-1];
	fseek(fp,0,SEEK_END);
	fwrite(&a,sizeof(a),1,fp);
	fclose(fp);
	printf("The record is successfully saved\n");
	printf("Save any more?(Y / N):");
	if(getch()=='n')
	    mainmenu();
	else
	    additem();
	}
}
void deleteitem()    //function that delete items from file fp
{
    int d;
    char another='y';
    while(another=='y')  //infinite loop
    {
	printf("Enter the Item ID to  delete:");
	scanf("%d",&d);
	fp=fopen("abc.txt","rb+");
	rewind(fp);
	while(fread(&a,sizeof(a),1,fp)==1)
	{
	    if(a.id==d)
	    {

		printf("\nThe item record is available:\n");
		printf("Item name is: %s\n",a.itemname);
		finditem='t';
	    }
	}
	if(finditem!='t')
	{
	    printf("No record is found modify the search");
	    if(getch())
	    mainmenu();
	}
	if(finditem=='t' )
	{
	    printf("Do you want to delete it?(Y/N):");
	    if(getch()=='y')
	    {
		ft=fopen("test.txt","wb+");  //temporary file for delete
		rewind(fp);
		while(fread(&a,sizeof(a),1,fp)==1)
		{
		    if(a.id!=d)
		    {
			fseek(ft,0,SEEK_CUR);
			fwrite(&a,sizeof(a),1,ft); //write all in tempory file except that
		    }                              //we want to delete
		}
		fclose(ft);
		fclose(fp);
		remove("abc.txt");
		rename("test.txt","abc.txt"); //copy all item from temporary file to fp except that
		fp=fopen("abc.txt","rb+");    //we want to delete
		if(finditem=='t')
		{
		    printf("The record is successfully deleted\n");
		    printf("Delete another record?(Y/N)");
		}
	    }
	else
	mainmenu();
	fflush(stdin);
	another=getch();
	}
	}
    mainmenu();
}
void searchitem()
{
    int d;
    printf("Search Item\n");
    printf("\t\t\t\t\t 1.Search By ID\n");
    printf("\t\t\t\t\t 2.Search By Name\n");
    printf("\t\t\t\t\t Enter Your Choice\n");
    fp=fopen("abc.txt","rb+"); //open file for reading propose
    rewind(fp);   //move pointer at the begining of file
    switch(getch())
    {
	  case '1':
	{
	    printf("\t\t\t\t\t Search Item By Id\n");
	    printf("\t\t\t\t\t Enter the Item id:");
	    scanf("%d",&d);
	    printf("\t\t\t\t\t \nSearching........\n");
	    while(fread(&a,sizeof(a),1,fp)==1)
	    {
		if(a.id==d)
		{
		    printf("\nThis Item is available\n");
		    printf("ID:%d\n",a.id);
		    printf("Name:%s\n",a.itemname);
		    printf("Qantity:%d \n",a.quantity);
		    printf("Price:Rs.%.2f\n",a.Price);
		    finditem='t';
		}

	    }
	    if(finditem!='t')  //checks whether conditiion enters inside loop or not
	    {
	    printf("\aNo Record Found\n");
	    }
	    printf("Try another search?(Y/N)");
	    if(getch()=='y')
	    searchitem();
	    else
	    mainmenu();
	    break;
	}
	case '2':
	{
	    char s[15];
	    printf("Search Item By Name\n");
	    printf("Enter Item Name:");
	    scanf("%s",s);
	    int d=0;
	    while(fread(&a,sizeof(a),1,fp)==1)
	    {
		if(strcmp(a.itemname,(s))==0) //checks whether a.name is equal to s or not
		{
		    printf("This Item is available\n");
		    printf("ID:%d\n",a.id);
		    printf("Name:%s\n",a.itemname);
		    printf("Quantity:%d\n",a.quantity);
		    printf("Price:Rs.%.2f\n",a.Price);
		    d++;
		}

	    }
	    if(d==0)
	    {
	    printf("\aNo Record Found\n");
	    }
	    printf("Try another search?(Y/N)\n");
	    if(getch()=='y')
	    searchitem();
	    else
	    mainmenu();
	    break;
	}
	default :
	getch();
	searchitem();
    }
    fclose(fp);
}
void edititem(void)  //edit information about book
{
	int c=0;
	int d,e;
	printf("Edit Items Section");
	char another='y';
	while(another=='y')
	{
	
		printf("Enter Item Id to be edited:");
		scanf("%d",&d);
		fp=fopen("abc.txt","rb+");
		while(fread(&a,sizeof(a),1,fp)==1)
		{
			if(checkid(d)==0)
			{
				printf("The Item is availble\n");
				printf("The Item ID:%d\n",a.id);
				printf("Enter new name:");
				scanf("%s",a.itemname);
				printf("Enter new quantity:");
				scanf("%d",&a.quantity);
				printf("Enter new price:");
				scanf("%f",&a.Price);
				printf("The record is modified");
				fseek(fp,ftell(fp)-sizeof(a),0);
				fwrite(&a,sizeof(a),1,fp);
				fclose(fp);
				c=1;
			}
			if(c==0)
			{
				printf("No record found");
			}
		}
		printf("Modify another Record?(Y/N)");
		fflush(stdin);
		another=getch() ;
	}
		returnfunc();
}
void returnfunc(void)
{
    {
	printf(" Press ENTER to return to main menu");
    }
    a:
    if(getch()==13) //allow only use of enter
    mainmenu();
    else
    goto a;
}
int getdata()
{
	int t;
	printf("Enter the Information Below\n");
	printf("Category:\n");
	printf("%s\n",catagories[s-1]);
	printf("Item ID:\t");
	scanf("%d",&t);
	if(checkid(t) == 0)
	{
		printf("\aThe Item ID already exists\a");
		getch();
		mainmenu();
		return 0;
	}
	a.id=t;
	printf("Item Name:");
	scanf("%s",a.itemname);
	printf("Quantity:");
	scanf("%d",&a.quantity);
	printf("Price:");
	scanf("%f",&a.Price);
	return 1;
}
int checkid(int t)  //check whether the book is exist in library or not
{
	rewind(fp);
	while(fread(&a,sizeof(a),1,fp)==1)
	if(a.id==t)
	return 0;  //returns 0 if book exits
    return 1; //return 1 if it not
}
//End of program
