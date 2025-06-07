# Task-3
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_ACCOUNTS 100

typedef struct {
    int id;
    char name[50];
    char type[10]; // "Savings" or "Current"
    float balance;
} Account;

Account accounts[MAX_ACCOUNTS];
int accountCount = 0;

// Function prototypes
void createAccount();
void deposit();
void withdraw();
void checkBalance();
void calculateInterest();
void displayMenu();

int main() {
    int choice;
    do {
        displayMenu();
        printf("\nEnter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: createAccount(); break;
            case 2: deposit(); break;
            case 3: withdraw(); break;
            case 4: checkBalance(); break;
            case 5: calculateInterest(); break;
            case 0: printf("Exiting program.\n"); break;
            default: printf("Invalid choice! Try again.\n");
        }
    } while (choice != 0);

    return 0;
}

void displayMenu() {
    printf("\n--- Bank Account Management ---\n");
    printf("1. Create Account\n");
    printf("2. Deposit Money\n");
    printf("3. Withdraw Money\n");
    printf("4. Check Balance\n");
    printf("5. Calculate Interest\n");
    printf("0. Exit\n");
}

void createAccount() {
    if (accountCount >= MAX_ACCOUNTS) {
        printf("Cannot create more accounts.\n");
        return;
    }

    Account acc;
    acc.id = accountCount + 1;

    printf("Enter customer name: ");
    scanf(" %[^\n]", acc.name);

    printf("Enter account type (Savings/Current): ");
    scanf("%s", acc.type);

    printf("Enter initial deposit amount: ");
    scanf("%f", &acc.balance);

    accounts[accountCount++] = acc;
    printf("Account created successfully! Account ID: %d\n", acc.id);
}

int findAccountById(int id) {
    for (int i = 0; i < accountCount; i++) {
        if (accounts[i].id == id)
            return i;
    }
    return -1;
}

void deposit() {
    int id;
    float amount;

    printf("Enter Account ID: ");
    scanf("%d", &id);
    int index = findAccountById(id);

    if (index == -1) {
        printf("Account not found!\n");
        return;
    }

    printf("Enter deposit amount: ");
    scanf("%f", &amount);
    if (amount < 0) {
        printf("Invalid amount.\n");
        return;
    }

    accounts[index].balance += amount;
    printf("Deposit successful. New balance: %.2f\n", accounts[index].balance);
}

void withdraw() {
    int id;
    float amount;

    printf("Enter Account ID: ");
    scanf("%d", &id);
    int index = findAccountById(id);

    if (index == -1) {
        printf("Account not found!\n");
        return;
    }

    printf("Enter withdrawal amount: ");
    scanf("%f", &amount);

    if (amount < 0 || amount > accounts[index].balance) {
        printf("Invalid or insufficient funds.\n");
        return;
    }

    accounts[index].balance -= amount;
    printf("Withdrawal successful. New balance: %.2f\n", accounts[index].balance);
}

void checkBalance() {
    int id;
    printf("Enter Account ID: ");
    scanf("%d", &id);

    int index = findAccountById(id);
    if (index == -1) {
        printf("Account not found!\n");
        return;
    }

    printf("Account Holder: %s\n", accounts[index].name);
    printf("Account Type : %s\n", accounts[index].type);
    printf("Balance      : %.2f\n", accounts[index].balance);
}

void calculateInterest() {
    int id;
    float rate, interest;
    printf("Enter Account ID: ");
    scanf("%d", &id);

    int index = findAccountById(id);
    if (index == -1) {
        printf("Account not found!\n");
        return;
    }

    if (strcmp(accounts[index].type, "Savings") == 0)
        rate = 0.04; // 4% interest
    else if (strcmp(accounts[index].type, "Current") == 0)
        rate = 0.02; // 2% interest
    else {
        printf("Unknown account type.\n");
        return;
    }

    interest = accounts[index].balance * rate;
    printf("Interest for Account ID %d (%.2f%%): %.2f\n", id, rate * 100, interest);
}
