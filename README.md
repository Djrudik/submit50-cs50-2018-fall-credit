#include <stdio.h>
#include <limits.h>

/*
4003600000000014         x16
. . . . . . . .
 400360000000001         x15
  . . . . . . .
   4003600000000         x13
    . . . . . .
*/

int main(void)
{
    long long int cc = 0; // cc number; could be 13, 15, 16 numbers
    int first_multiply = 0;
    int second_summ = 0;
    int final_summ = 0;

    printf("Enter your CC number:\n");

    do
    {
        scanf("%lld", &cc);
        while (getchar() != '\n') ; // clear buffer
    }
    while (cc < (long long)100000000);

    if (cc < (long long)1000000000000 || cc >= (long long)10000000000000000) // min/max CC number: error reporting
    {
        printf("INVALID\n");
        return 0;
    }

    // now get separate numbers (2nd starting from end)
    // make array which contain 9 elements (we will use only 8)
    int n[9];
    for (int i = 0; i < 10; i++)
    {
        n[i] = 0; // fill array with 0;
    }

    n[1] = (cc % 100)               / 10;
    n[2] = (cc % 10000)             / 1000;
    n[3] = (cc % 1000000)           / 100000;
    n[4] = (cc % 100000000)         / 10000000;
    n[5] = (cc % 10000000000)       / 1000000000;
    n[6] = (cc % 1000000000000)     / 100000000000;
    n[7] = (cc % 100000000000000)   / 10000000000000;
    n[8] = (cc % 10000000000000000) / 1000000000000000;

    // multiplied x2 (`d` - doubled)
    int d[9];
    for (int i = 0; i < 10; i++)
    {
        d[i] = 0; // fill array with 0;
    }

    d[1] = (n[1] * 2);
    d[2] = (n[2] * 2);
    d[3] = (n[3] * 2);
    d[4] = (n[4] * 2);
    d[5] = (n[5] * 2);
    d[6] = (n[6] * 2);
    d[7] = (n[7] * 2);
    d[8] = (n[8] * 2);

    // Extract number from 0 to 9 from doubled numbers with module (`m`).
    // Sometimes we will have 1 digit, sometimes 2 (then one will be 1).
    int m[9];
    for (int i = 0; i < 10; i++)
    {
        m[i] = 0; // fill array with 0;
    }

    for (int i = 1; i < 9; i++) // must lurk only at 8 (legit array)
    {
        if (d[i] > 10)
        {
            m[i] = (d[i] % 10) + 1; // as we need summ, we can add 1 right on
        }
        else if (d[i] == 10)
        {
            m[i] = 1;
        }
        else
        {
            m[i] = d[i];
        }
    }

    // first half done:
    first_multiply = m[1] + m[2] + m[3] + m[4] + m[5] + m[6] + m[7] + m[8];

    // now take leftover digits from initial CC number
    int x[9];
    for (int i = 0; i < 10; i++)
    {
        x[i] = 0; // fill array with 0;
    }

    x[1] = (cc % 10);
    x[2] = (cc % 1000)             / 100;
    x[3] = (cc % 100000)           / 10000;
    x[4] = (cc % 10000000)         / 1000000;
    x[5] = (cc % 1000000000)       / 100000000;
    x[6] = (cc % 100000000000)     / 10000000000;
    x[7] = (cc % 10000000000000)   / 1000000000000;
    x[8] = (cc % 1000000000000000) / 100000000000000;

    // done! now calculate:
    second_summ    = x[1] + x[2] + x[3] + x[4] + x[5] + x[6] + x[7] + x[8];
    final_summ     = first_multiply + second_summ;

    if (final_summ % 10 == 0)
    {
        if (cc < (long long)10000000000000)    // 13 digits
        {
            printf("VISA\n");
        }

        else if (cc < (long long)1000000000000000 && // 15 digits
                 ((cc >= (long long)340000000000000 && cc < (long long)350000000000000) || // starts with 34
                  (cc >= (long long)370000000000000 && cc < (long long)380000000000000)))  // starts with 37
        {
            printf("AMEX\n");
        }

        else if (cc < (long long)10000000000000000) // 16 digits
        {
            if (n[8] == 4) // visa cc always starts with 4
            {
                printf("VISA\n");
            }
            else if (cc >= (long long)5100000000000000 && cc < (long long)5600000000000000) // MCARD starts with 51,52,53,54,55
            {
                printf("MASTERCARD\n");
            }
            else
            {
                printf("INVALID\n");
            }
        }
        else
        {
            printf("INVALID\n");
        }
    }
    else
    {
        printf("INVALID\n");
    }

    return 0;
}
