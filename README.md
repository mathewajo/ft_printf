# ft_printf

***125/100***

![Screenshot 2021-11-20 at 09 54 29](https://user-images.githubusercontent.com/76873228/142720762-b79ac4a2-b890-4943-829a-ffbaf3eee237.jpg)




Tester Used:
https://github.com/Tripouille/printfTester

Note:
- Code attached as zip folder. Extract it and link it if you want to test.
- Should not submit main
- 
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
