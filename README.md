# basic-mini-electronic-dictionary
可自由輸入和刪除單字和其解釋意思，但功能非常基礎，不過有預留未來擴充功能

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_WORD_LEN 50  //定義一個名為MAX_WORD_LEN , 代表數字50
#define MAX_DEF_LEN 200  //同上 , 目的為看到 MAX_WORD_LEN , 就直接替換成 50 , 看到 MAX_DEF_LEN 就替換成 200。

typedef struct{

    char word[MAX_WORD_LEN];  //儲存英文單字
    char definition[MAX_DEF_LEN];  //儲存其定義

}entry;  //宣告此結構名稱為entry

#define MAX_ENTRIES 1000  //建立一個叫做 dictionary 的陣列 , 它可以儲存 MAX_ENTRIES 筆單字資料 , 每一筆資料的型別是 Entry。

    entry dictionary[MAX_ENTRIES];

    int entry_count = 0;

void menu(){

    printf("===== mini electronic dictionary menu ===== \n");
    printf("         1. Consult the dictionary \n");
    printf("           2. Add new vocabulary \n");
    printf("          3. Show all vocabularys \n");
    printf("           4. Delete a vocbulary \n");
    printf("            5. Exit the program \n");
    printf(" Please select the function (enter a number 1~5) \n");

}

void add(){

    if(entry_count >= MAX_ENTRIES){

        printf("Dictionary full! can't add more entries. \n");

        return;

    }

    char word[MAX_WORD_LEN];
    char def[MAX_DEF_LEN];

    printf("Enter new vocabulary:  \n");
    scanf("%s" , word);
    getchar();//用於吃掉上一行結束的enter

    printf("Enter the definition:  \n");
    fgets(def , MAX_DEF_LEN , stdin);  //用於可接受空格

    //存進dectionary陣列 , 因為是新增第xx筆資料 , 所以存在entry_count裡頭。
    strcpy(dictionary[entry_count].word , word);
    strcpy(dictionary[entry_count].definition , def);
    entry_count++;

    printf("Vocabulary added successfully \n");
}


void consult(){

    char vocabulary[MAX_WORD_LEN];

    printf("Please enter the vocabulary you want to search \n");
    scanf("%s", vocabulary);

    for(int i = 0; i < entry_count ; i++){

        if(strcmp(dictionary[i].word , vocabulary) == 0 ){

            printf("Definition: %s\n" , dictionary[i].definition);

            return;
        }

    }
}

void show(){

    if(entry_count == 0){

        printf("The dictionary is currently empty. \n");

        return;
    }

    printf("This is all vocabulary and the definition. \n");

    for(int i = 0; i < entry_count ; i++){

        printf("%s : %s\n" , dictionary[i].word , dictionary[i].definition);
    }
}

void out(){

    printf("Thank you for using , you have exit the dictionary. \n");

    exit(0);
}

void del(){

    if(entry_count == 0){

        printf("The dictionary is empty , nothing to delete. \n");

        return;
    }

    char target[MAX_WORD_LEN];

    printf("Enter the vocabulary you want to delete: \n");
    scanf("%s" , &target);

    for(int i = 0; i < entry_count; i ++){    // 使用 for 迴圈尋找目標單字

        if(strcmp(dictionary[i].word , target) == 0){    // 比對當前單字與輸入的單字是否相同（strcmp 回傳 0 表示相等）

            for(int j = i; j < entry_count --; j++){    // 如果找到，進行刪除並將後面的單字往前移

                dictionary[j] = dictionary[j + 1];
            }

            entry_count --;    // 將字典中的單字數量減一，因為刪掉一個

            printf("Vocabulary '%s' delete successfully. \n" , target);

            return;    //找到並刪除後返回，不再繼續搜尋
        }
    }

    printf("Vocabulary not found , cannot delete.");    //迴圈跑完後未搜尋到單字，直接顯示搜尋無果
}

void run(){

    int choice;

    while(1){

        menu();

        printf("\n");
        scanf("%d" , &choice);

        if(choice == 1){

            consult();
        }

        else if(choice == 2){

            add();
        }

        else if(choice == 3){

            show();
        }

        else if(choice == 4){

            del();
        }

        else if(choice == 5){

            out();
        }

        else{

            printf("invalid option, please re_enter \n");
        }

        int cont_choice;

        printf("Do you want to continue to operate the dictionary? [YES : 1 / NO : 0]  \n");
        scanf("%d" , &cont_choice);

        if(cont_choice == 0){

            out();
        }

    }
}

int main(){

    run();

    return 0;
}
