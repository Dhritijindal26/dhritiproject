#dhritiproject


#include<stdio.h>
#include<conio.h>
#include<string.h>

void main()
{
    char arr[10],b[10],label[10],opcode[10],operand[10],symbol[10],choice;
    int start,difference,i,address,b2,length,act_len,fin_ad,prev_ad,j=0;
    char mnemonic[15][15]={"LDE","STO","LDEH","STOH"};
    char code[15][15]={"20","35","48","60"};
    FILE *fp1,*fp2,*fp3,*fp4;

    fp4=fopen("INTERMEDIATE.DAT","r");
    fscanf(fp4,"%s%s%s",label,opcode,operand);

   while(strcmp(opcode,"END")!=0)
   {
    prev_ad=address;
    fscanf(fp4,"%d%s%s%s",&address,label,opcode,operand);
   }
   fin_ad=address;
   fclose(fp4);
   fp4=fopen("INTERMEDIATE.DAT","r");
   fp1=fopen("ASSEMBLYLIST.DAT","w");
   fp3=fopen("OBJECTCODE.DAT","w");
   fscanf(fp4,"%s%s%s",label,opcode,operand);
   if(strcmp(opcode,"START")==0)
   {
     fprintf(fp1,"\t%s\t%s\t%s\n",label,opcode,operand);
     fprintf(fp3,"H^%s^00%s^00%d\n",label,operand,finaddr);
     fscanf(fp4,"%d%s%s%s",&address,label,opcode,operand);
     start=address;
     difference=prev_ad-start;
     fprintf(fp3,"T^00%d^%d",address,difference);
   }
   do
   {
    if(strcmp(opcode,"BYTE")==0)
    {
     fprintf(fp1,"%d\t%s\t%s\t%s\t",address,label,opcode,operand);
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
   }while(strcmp(opcode,"END")!=0)
}

