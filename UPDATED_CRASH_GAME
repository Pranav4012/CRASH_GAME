#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_USERS 100
#define UNAME_LEN 50
#define PASS_LEN 50

typedef struct
{
    char uname[UNAME_LEN];
    char pass[PASS_LEN];
    float balance;
    float earnings;
    float losses;
} User;

// func prototypes
int reg_usr(User *u, int *c);
int login_usr(User *u, int c);
float gen_mult();
void play_game(User *u);
void save_usr(User *u, int c);
int load_usr(User *u);
void chat_resp();
void show_guide();
void show_strat();
void show_strat1();
void show_strat2();
void show_strat3();

int main(void)
{
    srand(time(NULL)); // seed RNG
    User u[MAX_USERS];
    int c = load_usr(u); // load users from file
    int logged_in_idx = -1;
    int choice;

    while (1)
    {
        printf("\n1. Register\n");
        printf("2. Login\n");
        printf("3. Exit\n");
        printf("4. Guide\n");
        printf("5. Strategy\n");
        printf("choose an option: ");
        scanf("%d", &choice);

        switch (choice)
        {
        case 1:
            reg_usr(u, &c); // register user
            break;
        case 2:
            logged_in_idx = login_usr(u, c);
            if (logged_in_idx != -1)
            {
                play_game(&u[logged_in_idx]); // start game
            }
            break;
        case 3:
            save_usr(u, c); // save users
            printf("had fun? :)\n");
            return 0;
        case 4:
            show_guide(); // show guide
            break;
        case 5:
            show_strat(); // show strategy
            break;
        default:
            printf("invalid option. try again.\n");
        }
    }
}

int reg_usr(User *u, int *c)
{
    if (*c >= MAX_USERS)
    {
        printf("user limit hit.\n");
        return 0;
    }

    User new_usr;
    printf("enter username: ");
    scanf("%s", new_usr.uname);
    printf("enter password: ");
    scanf("%s", new_usr.pass);

    new_usr.balance = 10000.0;
    new_usr.earnings = 0.0;
    new_usr.losses = 0.0;

    u[*c] = new_usr;
    (*c)++;

    printf("reg complete. initial balance: $%.2f\n", new_usr.balance);
    return 1;
}

int login_usr(User *u, int c)
{
    char uname[UNAME_LEN];
    char pass[PASS_LEN];
    int choice;

    printf("enter username: ");
    scanf("%s", uname);
    printf("enter password: ");
    scanf("%s", pass);

    for (int i = 0; i < c; i++)
    {
        if (strcmp(u[i].uname, uname) == 0 && strcmp(u[i].pass, pass) == 0)
        {
            printf("logged in! welcome, %s.\n", uname);
            printf("balance: $%.2f\n\n", u[i].balance);

            printf("1. play game\n");
            printf("2. menu\n");
            printf("choose: ");
            scanf("%d", &choice);

            if (choice == 1)
            {
                return i; // return user idx to play
            }
            else
            {
                return -1; // return to menu
            }
        }
    }

    printf("invalid login.\n");
    return -1; // login fail
}

float gen_mult()
{
    float rand_val = (float)rand() / (float)RAND_MAX; // uniform(0,1)
    if (rand_val == 0)
    {
        rand_val = 0.0001; 
    }
    return 1.0 / rand_val; 
}

void chat_resp()
{
    const char *msgs[] = {
        "good luck!",
        "hope for big multipliers!",
        "timing is key!",
        "go big or go home!",
        "don't cash out too late!",
        "you got this!",
        "good vibes, let's win!",
        "let's crush it!",
        "cash out wisely!"
    };

    int idx = rand() % (sizeof(msgs) / sizeof(msgs[0]));
    printf("chatbot: %s\n", msgs[idx]);
}

void show_guide()
{
    printf("\n--- game guide ---\n");
    printf("this is a crash betting game. place your bet, set a multiplier goal, and cash out before the multiplier crashes.\n");
    printf("if you cash out before the crash, you win. if the multiplier crashes before you cash out, you lose.\n");
    printf("play responsibly!\n\n");
}

void show_strat()
{
    int choice;
    printf("\n--- choose a strategy ---\n");
    printf("1. low-risk, steady wins (early cash-out)\n");
    printf("2. high-risk, high-reward (late cash-out)\n");
    printf("3. martingale (double bets after losses)\n");
    printf("pick a strat (1-3): ");
    scanf("%d", &choice);

    switch (choice)
    {
    case 1:
        printf("\nlow-risk strategy (early cash-out)\n");
        break;
    case 2:
        printf("\nhigh-risk strategy (late cash-out)\n");
        break;
    case 3:
        printf("\nmartingale strategy\n");
        break;
    default:
        printf("invalid choice.\n");
        break;
    }
}

void play_game(User *u)
{
    while (1)
    {
        float bet, cash_out;
        float mult = 1.0;
        float crash_point;
        clock_t start, current;
        int choice;
        int cashed_out = 0;

        printf("your balance: $%.2f\n", u->balance);

        printf("enter bet amount: ");
        scanf("%f", &bet);

        if (bet > u->balance)
        {
            printf("not enough balance.\n");
            return;
        }

        printf("enter cash-out multiplier: ");
        scanf("%f", &cash_out);

        crash_point = gen_mult();
        chat_resp(); // pre-game chat

        start = clock();
        while (mult < crash_point)
        {
            printf("multiplier: %.2f\n", mult);
            if (mult >= cash_out)
            {
                cashed_out = 1; // cashed out
                break;
            }

            current = clock();
            while ((double)(clock() - current) / CLOCKS_PER_SEC < 1.0)
            {
            }

            mult += 0.1;
        }

        printf("crashed at %.2f.\n", crash_point);

        if (cashed_out)
        {
            float winnings = (bet * cash_out) - bet;
            u->balance += winnings;
            u->earnings += winnings;
            printf("cashed out at %.2f. won $%.2f. new balance: $%.2f\n", 
                   cash_out, winnings, u->balance);
        }
        else
        {
            u->balance -= bet;
            u->losses += bet;
            printf("lost $%.2f. new balance: $%.2f\n", 
                   bet, u->balance);
        }

        chat_resp(); // post-game chat

        printf("\n1. continue?\n");
        printf("2. return to menu\n");
        printf("choose: ");
        scanf("%d", &choice);

        if (choice != 1)
        {
            return;
        }
    }
}

void save_usr(User *u, int c)
{
    FILE *f = fopen("users.dat", "wb");
    if (f == NULL)
    {
        printf("save error.\n");
        return;
    }
    fwrite(u, sizeof(User), c, f);
    fclose(f);
    printf("user data saved.\n");
}

int load_usr(User *u)
{
    FILE *f = fopen("users.dat", "rb");
    if (f == NULL)
    {
        return 0; // no users found
    }
    int c = fread(u, sizeof(User), MAX_USERS, f);
    fclose(f);
    printf("loaded %d users.\n", c);
    return c;
}
