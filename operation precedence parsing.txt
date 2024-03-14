#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
int isOperator(char ch) {
    return (ch == '+' || ch == '-' || ch == '*' || ch == '/' || ch == '^');
}

int getPrecedence(char op) {
    if (op == '^') return 3;
    if (op == '*' || op == '/') return 2;
    if (op == '+' || op == '-') return 1;
    return 0;
}

void parseExpression(char *expression) {
    char stack[100];
    int top = -1;

    printf("Operator Precedence Parsing:\n");

    for (int i = 0; expression[i] != '\0'; i++) {
        if (isalnum(expression[i])) {
            printf("%c ", expression[i]); 
        } else if (isOperator(expression[i])) {
            while (top >= 0 && getPrecedence(stack[top]) >= getPrecedence(expression[i])) {
                printf("%c ", stack[top--]);
            }
            stack[++top] = expression[i];
        } else if (expression[i] == '(') {
            stack[++top] = expression[i];
        } else if (expression[i] == ')') {
            while (top >= 0 && stack[top] != '(') {
                printf("%c ", stack[top--]);
            }
            if (top >= 0 && stack[top] == '(') {
                top--;
            }
        }
    }
    while (top >= 0) {
        printf("%c ", stack[top--]);
    }

    printf("\n");
}

int main() {
    char expression[100];
    printf("Enter an arithmetic expression: ");
    scanf("%s", expression);
    parseExpression(expression);

    return 0;
}
