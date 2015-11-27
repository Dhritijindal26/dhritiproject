#dhritiproject

#include<stdio.h>
#include<conio.h>
#include<string.h>

void main()
{ 
    char arr[10],b[10],label[10],opcode[10],operand[10],symbol[10],ch;
   unsigned int start,difference,i,address,b2,length,act_len,fin_ad,prev_ad,j=0,choice;
    long long int k,z=0;
    char mnemonic[15][15]={"LDE","STO","LDEH","STOH"};
    char code[15][15]={"20","35","48","60"};
    FILE *fp1,*fp2,*fp3,*fp4;

    fp4=fopen("INTERMEDIATE.DAT","r");
    fscanf(fp4,"%s%s%s",label,opcode,operand);

   while(strcmp(opcode,"END")!=0)
   {
    prev_ad=address;
    fscanf(fp4,"%x%s%s%s",&address,label,opcode,operand);
   }
   fin_ad=address;
   fclose(fp4);
   fp4=fopen("INTERMEDIATE.DAT","r");
   fp1=fopen("ASSEMBLYLIST.DAT","w");
   fp3=fopen("OBJECTCODE.DAT","w");
   fp2=fopen("SYMBOLTABLE.DAT","r");
   fscanf(fp4,"%s%s%s",label,opcode,operand);
   if(strcmp(opcode,"START")==0)
   {
     fprintf(fp1,"\t%s\t%s\t%s\n",label,opcode,operand);
     fprintf(fp3,"H^%s^00%s^00%x\n",label,operand,fin_ad);
     fscanf(fp4,"%x%s%s%s",&address,label,opcode,operand);
     start=address;
     difference=prev_ad-start;
     fprintf(fp3,"T^00%x^%x",address,difference);
   }
   do
   {
    if(strcmp(opcode,"BYTE")==0)
    {
     fprintf(fp1,"%x\t%s\t%s\t%s\t",address,label,opcode,operand);
     length=strlen(operand);
     act_len=length-3;
     fprintf(fp3,"^");
     for(i=2;i<(act_len+2);i++)
     {
      itoa(operand[i],b,16);
      fprintf(fp1,"%s",b);
      fprintf(fp3,"%s",b);
     }
     fprintf(fp1,"\n");
    }
    else if(strcmp(opcode,"WORD")==0)
    {
     length=strlen(operand);
     itoa(atoi(operand),arr,10);
     fprintf(fp1,"%x\t%s\t%s\t%s\t00000%s\n",address,label,opcode,operand,arr);
     fprintf(fp3,"^00000%s",arr);
    }
     else if((strcmp(opcode,"RESB")==0)||(strcmp(opcode,"RESW")==0))
     fprintf(fp1,"%x\t%s\t%s\t%s\n",address,label,opcode,operand);
     else
     {
      while(strcmp(opcode,mnemonic[j])!=0)
      j++;
      if(strcmp(operand,"COPY")==0)
       fprintf(fp1,"%x\t%s\t%s\t%s\t%s0000\n",address,label,opcode,operand,code[j]);
      else
      {
        rewind(fp2);
        fscanf(fp2,"%s%x",symbol,&b2);
        while(strcmp(operand,symbol)!=0)
        fscanf(fp2,"%s%x",symbol,&b2);
        fprintf(fp1,"%x\t%s\t%s\t%s\t%s%X\n",address,label,opcode,operand,code[j],b2);
        fprintf(fp3,"^%s%x",code[j],b2);
      }
     }
     fscanf(fp4,"%x%s%s%s",&address,label,opcode,operand);
   }while(strcmp(opcode,"END")!=0);
   fprintf(fp1,"%x\t%s\t%s\t%s\n",address,label,opcode,operand);
   fprintf(fp3,"\nE^00%x",start);
 // printf("\n Intermediate file is converted into object code");
  fclose(fp3);
  fclose(fp4);
  fclose(fp1);
      printf("************************************ASSEMBLER***********************************\n");
      system("color 5");
      printf("Press Enter to continue.........");
      getchar();
  do
  {
      printf("\n\nEnter your choice\n\t\t\tPress 1 to show the program\n\t\t\tPress 2 to check Symbol Table\n\t\t\tPress 3 to calculate object file\n\t\t\tPress 4 to check output file\n\t\t\tPress 5 to exit\n\n");
      scanf("%d",&choice);
      switch(choice)
      {
       case 1:
        fp4=fopen("INTERMEDIATE.DAT","r");
        ch=fgetc(fp4);
        while(ch!=EOF)
        {
         printf("%c",ch);
         ch=fgetc(fp4);
        }
        break;
       case 2:
          fp2=fopen("SYMBOLTABLE.DAT","r");
          ch=fgetc(fp2);
          while(ch!=EOF)
          {
           printf("%c",ch);
           ch=fgetc(fp2);
          }
          break;
      case 3:
        for(k=0;k<50000000;k++)
        {
         // printf("Object file calculated");
         z++;
        }
        printf("Object file calculated\n\n");
        fp3=fopen("OBJECTCODE.DAT","r");
        ch=fgetc(fp3);
        while(ch!=EOF)
        {
         printf("%c",ch);
         ch=fgetc(fp3);
        }
        fclose(fp3);
        break;
      case 4:
       fp1=fopen("ASSEMBLYLIST.DAT","r");
       ch=fgetc(fp1);
       while(ch!=EOF)
       {
        printf("%c",ch);
        ch=fgetc(fp1);
       }
        break;
      case 5:
        printf("\n\t\t\t\t by DIVYA JAIN\n\t\t\t\t    DHRITI JINDAL");
        exit(0);

    }

  }while(choice<6);
}
/*ASSEMBLYLIST.DAT
COPY	START	2000
2000	**	LDE	FIVE	20200F
2003	**	STO	ALPHA	35200C
2006	**	LDEH	CHARZ	482012
2009	**	STOH	C1	602015
200c	ALPHA	RESW	1
200f	FIVE	WORD	5	000005
2012	CHARZ	BYTE	C'EOF'	454f46
2015	C1	RESB	1
2016	**	END	**
*/
/*INTERMEDIATE.DAT
COPY    START    2000
2000    **    LDE    FIVE
2003    **    STO    ALPHA
2006    **    LDEH    CHARZ
2009    **    STOH    C1
200C    ALPHA    RESW    1
200F    FIVE    WORD    5
2012    CHARZ    BYTE    C'EOF'
2015    C1    RESB    1
2016    **    END    ** */

/*H^COPY^002000^002016
T^002000^15^20200f^35200c^482012^602015^000005^454f46
E^002000 */


/*SYMBOLTABLE.DAT
ALPHA    200C
FIVE    200F
CHARZ    2012
C1    2015 */
