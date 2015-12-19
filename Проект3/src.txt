#define _CRT_SECURE_NO_WARNINGS
#include <locale.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#include "search.h"

void getTokens();
char *getTokenType(char *thisToken);
int isDelim(char c);

char *prog;

void main() {
	setlocale(LC_ALL, "RUS");
	char fromFile[3] = { "\0" };
	char polynomText[30] = { "\0" };
	FILE *f;

	f = fopen("newFile.txt", "r");

	while (!feof(f)) {
		fscanf(f, "%c", &fromFile);
		strcat(polynomText, fromFile);
	}

	polynomText[strlen(polynomText) - 1] = NULL;
	prog = &polynomText;

	fclose(f);

	printf("%s\n", prog);
	getTokens();

	expressionIsTrue();

	system("pause");
}

void getTokens() {
	char temp[30] = { "\0" }, newString[300] = { "\0" }, *token;
	int counter = strlen(prog), j = 0;
	printf("\nALL TOKENS:\n\n");
	for (int i = 0; i < counter; i++) {
		while (*prog && !(isspace(*prog)) && !(isDelim(*prog)) && isdigit(*prog)) {
			temp[j] = *prog;
			*prog++;
			j++; i++;
		}
		if (strlen(temp) > 0) {
			strcpy(myTokens[countTokens].tokenName, &temp);
			countTokens++;
		}
		memset(temp, 0, sizeof(temp));
		if (!isdigit(*prog) && !isDelim(*prog) && ((int)(*prog) != 0) && !(isspace(*prog))) {
			temp[0] = *prog;
			if ((int)temp[0] > 0) {
				strcpy(myTokens[countTokens].tokenName, &temp);
				countTokens++;
				memset(temp, 0, sizeof(temp));
			}
		}
		if (isDelim(*prog) && ((int)(*prog) != 0)) {
			temp[0] = *prog;
			strcpy(myTokens[countTokens].tokenName, &temp);
			countTokens++;
			memset(temp, 0, sizeof(temp));
		}
		*prog++;
		j = 0;
	}

	for (int i = 0; i < countTokens; i++) {
		printf("<%s>", myTokens[i].tokenName, i);
		strcpy(myTokens[i].tokenType, getTokenType(myTokens[i].tokenName));
		printf("[%s:%d] ", myTokens[i].tokenType, i);
	}
	printf("\n\n");
}

int isDelim(char c) {
	if (strchr("^+-/*=", c))
		return 1;
	return 0;
}

char *getTokenType(char *thisToken) {
	int flag = 1;
	if ((strcmp(thisToken, "-")) == 0)
		return "MINUS";
	if ((strcmp(thisToken, "+")) == 0)
		return "PLUS";
	if ((strcmp(thisToken, "*")) == 0)
		return "MUL";
	if ((strcmp(thisToken, "/")) == 0)
		return "DIV";
	if ((strcmp(thisToken, "^")) == 0)
		return "POWER";
	if ((strcmp(thisToken, "=")) == 0)
		return "EQUAL";
	for (int i = 0; i < strlen(thisToken); i++) {
		if (!isdigit(thisToken[i]))
			flag = 0;
	}
	if (flag == 1)
		return "CONST";
	return "VAR";
}