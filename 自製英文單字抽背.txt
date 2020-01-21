#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <windows.h>

void SetColor(unsigned short ForeColor,unsigned short BackGroundColor)
{HANDLE hCon=GetStdHandle(STD_OUTPUT_HANDLE);
SetConsoleTextAttribute(hCon,(ForeColor%16)|(BackGroundColor%16*16));}//字體顏色控制函式

int index=-1;
char eng_voc[2000][20]={0};
char chi_voc[2000][20]={0};

void file_load() //載入檔案
{
    index=-1;
    system("CLS");
    FILE *dict;
    dict=fopen("c:\\english_voc.txt","r");
    if(dict == NULL){
        SetColor(30,0);
        printf("Unable to find the file. Please check if you put the file into your C drive.\nThen named the file 'english_voc.txt'\n");
        SetColor(7,0);
        system("pause");
        exit(1);}
    while(!feof(dict)){
        if(index>1999){
            printf("超出單字量");
            system("pause");
            exit(1);
        }
        index++;
        fscanf(dict, "%s %s",&eng_voc[index],&chi_voc[index]);
        printf("%d.%s\t%s\n",index+1,eng_voc[index],chi_voc[index]);
    }
    SetColor(10,0);
    fclose(dict);
    printf("----------------------\nLoading Successful!\n\t(tony11306自製)\n----------------------\n");
    printf("\nword volume:%d/2000\n\n",index+1);
    SetColor(7,0);
}

/*void eng_multi_choice_quest()
{


}*/

void eng_quest() //英文翻中文
{
        char user_type[20]={0};
        int random_number;
        random_number=rand()%(index+1);
        SetColor(15,0);
        printf("Question:%s\n\n",eng_voc[random_number]);
        printf("Please type your answer(Chinese answer):");
        SetColor(27,0);
        scanf("%s",&user_type);

        if(strcmp(user_type,chi_voc[random_number])==0){
            SetColor(10,0);
            printf("--------------\nCorrect Answer!\n--------------\n");
            SetColor(7,0);
        }
        else{
            SetColor(12,0);
            printf("--------------\nWrong Answer\nThe True Answer Is:%s\n--------------\n",chi_voc[random_number]);
            SetColor(7,0);
        }
}

void chi_quest() //中文翻英文
{
        char user_type[20]={0};
        int random_number;
        random_number=rand()%(index+1);
        SetColor(15,0);
        printf("Question:%s\n\n",chi_voc[random_number]);
        printf("Please type your answer(English answer):");
        SetColor(27,0);
        scanf("%s",&user_type);
        if(strcmp(user_type,eng_voc[random_number])==0){
            SetColor(10,0);
            printf("--------------\nCorrect Answer!\n--------------\n");
            SetColor(7,0);
        }
        else{
            SetColor(12,0);
            printf("--------------\nWrong Answer\nThe True Answer Is:%s\n--------------\n",eng_voc[random_number]);
            SetColor(7,0);
        }
}

int main()
{
    int i;
    int user_choice;

    file_load();

    srand(time(NULL));

    while(1){
        SetColor(5,0);
        printf("Press「1」 if you want to translate English into Chinese\nPress「2」 if you want to translate Chinese into English\nPress「3」 if you want to quit\nPress「4」 to show the dictionary:\nPress「5」 to refresh\n");
        SetColor(7,0);
        fflush(stdin);
        scanf("%d",&user_choice);
        if(user_choice==1){
            system("CLS");
            eng_quest();
        }
        else if(user_choice==2){
            system("CLS");
            chi_quest();
        }
        else if(user_choice==3){
            exit(0);
        }
        else if(user_choice==4){
            system("CLS");
            SetColor(3,0);
            for(i=0;i<=index;i++){
                printf("%d.%s\t%s\n",i+1,eng_voc[i],chi_voc[i]);
            }
            SetColor(7,0);
        }
        else if(user_choice==5){
            file_load();
        }
    }
    return 0;
}
