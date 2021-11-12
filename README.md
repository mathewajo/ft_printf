# ft_printf

Instructions:

1. Clone the github
2. type **make re**
3. make a main 
4. include #include “ft_printf.h” in the main file
5. gcc -L . -lftprintf main.c
6. ./a.out

Please note: I need to do **make re** every time I make changes in the code.


check this:

int main(void)
{
    int number;

    number = 42;
    ft_printf("%-05d\n", number);
    printf("%-05d\n", number);
    return (0);
}
