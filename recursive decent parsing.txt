#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

// Function prototypes
void E();
void E_prime();
void T();
void T_prime();
void F();

// Input expression
char input[100];
// Index to traverse the input
int idx = 0;

// Function to handle errors
void error() {
    printf("Error in parsing the expression.\n");
    exit(1);
}

// Function to match a terminal symbol
void match(char symbol) {
    if (input[idx] == symbol) {
        idx++;
    } else {
        error();
    }
}

// E ? TE'
void E() {
    T();
    E_prime();
}

// E' ? +TE' | e
void E_prime() {
    if (input[idx] == '+') {
        match('+');
        T();
        E_prime();
    }
    // epsilon case (do nothing)
}

// T ? FT'
void T() {
    F();
    T_prime();
}

// T' ? *FT' | e
void T_prime() {
    if (input[idx] == '*') {
        match('*');
        F();
        T_prime();
    }
    // epsilon case (do nothing)
}

// F ? (E) | id
void F() {
    if (input[idx] == '(') {
        match('(');
        E();
        match(')');
    } else if (isalnum(input[idx])) {
        match(input[idx]);
    } else {
        error();
    }
}

int main() {
    printf("Enter an arithmetic expression: ");
    scanf("%s", input);

    // Start parsing
    E();

    // Check if the entire input is consumed
    if (input[idx] == '\0') {
        printf("The expression is successfully parsed.\n");
    } else {
        printf("Unexpected symbols appears in the expression.\n");
    }

    return 0;
}
