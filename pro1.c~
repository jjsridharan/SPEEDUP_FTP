#if defined(__unix__) || defined(__VMS)
#include <unistd.h>
#endif
#if defined(_WIN32)
#include <windows.h>
#endif
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <ctype.h>
#if defined(__unix__)
#include <sys/time.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <netdb.h>
#include <arpa/inet.h>
#elif defined(VMS)
#include <types.h>
#include <socket.h>
#include <in.h>
#include <netdb.h>
#include <inet.h>
#elif defined(_WIN32)
#include <winsock.h>
#endif

#define BUILDING_LIBRARY

#if defined(_WIN32)
#define SETSOCKOPT_OPTVAL_TYPE (const char *)
#else
#define SETSOCKOPT_OPTVAL_TYPE (void *)
#endif

#define FTPLIB_BUFSIZ 8192
#define ACCEPT_TIMEOUT 30

#define FTPLIB_CONTROL 0
#define FTPLIB_READ 1
#define FTPLIB_WRITE 2

#if !defined FTPLIB_DEFMODE
#define FTPLIB_DEFMODE FTPLIB_PASSIVE
#endif
#include "zip.h"
#include "zip.c"
#include "pro.c"

int main()
{
	netbuf *n;
	char ip[20],uname[25],passwd[25],wz[5],unzi[100]="sshpass -p ";
	char *ss[20];
	int nof,i,choice;
	for(i=0;i<20;i++)
	{
		ss[i]=(char *)malloc(20);
	}
	printf("----------------------------------------------------------------------------");
	printf("-----------------------------------SPEEDUP FTP--------------------------------------");
	printf("------------------------------------------------------------------------------");
	FtpInit();
	printf("\n\nEnter the IP Address : ");
	scanf("%s",ip);
	int s=FtpConnect(ip,&n);
	if(s==0)
	{
		printf("\n\nError in Connecting to : %s, Exiting\n",ip);
		exit(0);
	}
	else
	{
		printf("\n\n-------------Connected--------------\n\n");
	}	
	printf("\n\nEnter the User Name : ");
	scanf("%s",uname);
	printf("\n\nEnter the Password : ");
	scanf("%s",passwd);
	int k=FtpLogin(uname,passwd,n);
	if(k==0)
	{
		printf("\n\nInvalid username or password, Exiting\n\n");
		exit(0);
	}
	else
	{
		printf("\n\n-------------Successfully logged in--------------\n\n");
	}
	while(1)
	{
		printf("Enter your Choice:\n1.Upload Files\n2.Download File\n3.List Files\n4.Change Directory\n\n");
		printf("Choice : ");	
		scanf("%d",&choice);
		switch(choice)
		{
			case 1:
				printf("\n\nEnter the Number of files : ");
				scanf("%d",&nof);
				for(i=0;i<nof;i++)
				{
					char cc;
					printf("\n\nEnter the File Number %d : ",i+1);
					scanf("%c",&cc);
					ss[i]=(char *)malloc(20);
					scanf("%s",ss[i]);
					printf("\n%s",ss[i]);
				}
				printf("Zipping Files...");
				int hs=zip_create("hello.zip",ss,nof);
				if(hs<0)
				{
					printf("\n\nError in zipping, Exiting\n\n");
				}
				else
				{
					printf("\n\n-------------Files Zipped,Ready to Upload--------------\n\n");
				}
				int h=FtpPut("hello.zip","hello.zip",FTPLIB_IMAGE,n);
				if(s<0)
				{
					printf("\n\nError in Uploading, Exiting\n\n");
					exit(0);
				}
				else
				{
					printf("\n\n-------------Successfully Uploaded--------------\n\n");
				}
				strcat(unzi,passwd);
				strcat(unzi," ssh -t ");
				strcat(unzi,uname);
				strcat(unzi,"@");
				strcat(unzi,ip);
				strcat(unzi," unzip hello.zip | kill -9 -1; bash -l");
				printf("%s\n",unzi);
				system(unzi);
				break;
			case 2 :
				printf("\n\nEnter the Name of file : ");
				scanf("%s",ss[0]);
				h=FtpGet(ss[0],ss[0],FTPLIB_IMAGE,n);
				if(h==0)
				{
					printf("\nError in Retrieving file");
				}
				else
				{
					printf("\n\nSucessfully Retrieved");
				}
				break;
			case 3 :
				h=FtpDir("hi.txt","",n);
				system("cat hi.txt");
				break;
			case 4 :
				printf("Enter the Directory to change :");
				scanf("%s",ss[0]);
				h=FtpChdir(ss[0],n);
				h=FtpDir("hi.txt","",n);
				system("cat hi.txt");
		}
		printf("\nThanks for using our service\n");
	}
}
