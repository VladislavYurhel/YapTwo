#define MAX 100
int countTokens = 0, next = 0, result;

void expressionIsTrue();
int E();
int T();
int EXPR(); int EXPR1(); int EXPR2(); int EXPR3(); int EXPR4(); int EXPR5();
int SIGN();
int term(char *token); int termM(char *token);

struct tokensStruct {
	char tokenName[30];
	char tokenType[30];
} myTokens[MAX];

void expressionIsTrue() {
	result = E();
	char resultS[30] = { "\0" };
	if (result == 1) {
		strcpy(resultS, "Okay");
	}
	else
		strcpy(resultS, "Error");
	printf("Result: %s\n\n", resultS);
	if ((next < countTokens - 1) || (result == 0))
		printf("Error about [%d] token [%s]\n\n", next, myTokens[next].tokenName);
}

int E() {
	return T() && SIGN() && EXPR() && SIGN() && term("CONST") && term("EQUAL") && term("CONST");
}

int T() {
	int save = next;
	return (term("VAR") && term("POWER") && termM("3")) || (next = save, term("CONST") && term("VAR") && term("POWER") && termM("3"));
}

int EXPR() {
	int save = next;
	return EXPR3() || (next = save, EXPR2()) || (next = save, EXPR1()) || (next = save, EXPR4()) || (next = save, EXPR5());
}

int EXPR1() {
	return term("CONST") && term("VAR") && term("POWER") && term("CONST");
}

int EXPR2() {
	return term("CONST") && term("VAR") && term("POWER") && term("CONST") && SIGN() && EXPR();
}

int EXPR3() {
	return term("VAR") && term("POWER") && term("CONST");
}

int EXPR4() {
	return term("VAR") && term("POWER") && term("CONST") && SIGN() && EXPR();
}

int EXPR5() {
	int save = next;
	return (term("CONST") && term("VAR")) || (next = save, term("VAR"));
}

int SIGN() {
	int save = next;
	return term("MINUS") || (next = save, term("PLUS")) || (next = save, term("MULT")) || (next = save, term("DIV"));
}

int term(char *token) {
	return !strcmp(myTokens[next++].tokenType, token);
}

int termM(char *token) {
	return !strcmp(myTokens[next++].tokenName, token);
}