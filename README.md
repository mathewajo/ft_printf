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
```c
int main(void)
{
    int number;

    number = 42;
    ft_printf("%-05d\n", number);
    printf("%-05d\n", number);
    return (0);
}
```

```c
# Full Code (not formatted according to Norm!)

/* ************************************************************************** */
/*                                                                            */
/*                                                        :::      ::::::::   */
/*   ft_printf.c                                        :+:      :+:    :+:   */
/*                                                    +:+ +:+         +:+     */
/*   By: ajomathew <ajomathew@student.42.fr>        +#+  +:+       +#+        */
/*                                                +#+#+#+#+#+   +#+           */
/*   Created: 2021/11/19 17:45:55 by ajomathew         #+#    #+#             */
/*   Updated: 2021/11/20 10:20:44 by ajomathew        ###   ########.fr       */
/*                                                                            */
/* ************************************************************************** */

#include <stdio.h>
#include <stdlib.h>
#include <stdarg.h>
#include <stdbool.h>
#include <unistd.h>

/*ft_printf struct*/
typedef struct s_print
{
 va_list args;
 bool dash;
 bool zero;
 bool number;
 bool space;
 bool sign;
 bool width;
 size_t width_details;
 bool precision;
 size_t precision_details;
 int len_total;
}t_print;

/*libft struct*/
typedef struct s_list
{
 void *content;
 struct s_list *next;
}t_list;

/*libft functions*/
int ft_isalpha(int c);
int ft_isdigit(int c);
int ft_isalnum(int c);
int ft_isascii(int c);
int ft_isprint(int c);
size_t ft_strlen(const char *str);
void *ft_memset(void *b, int c, size_t len);
void ft_bzero(void *s, size_t n);
void *ft_memcpy(void *dst, const void *src, size_t n);
void *ft_memmove(void *dst, const void *src, size_t len);
size_t ft_strlcpy(char *dst, const char *src, size_t dstsize);
size_t ft_strlcat(char *dst, const char *src, size_t size);
int ft_toupper(int c);
int ft_tolower(int c);
char *ft_strchr(const char *s, int c);
char *ft_strrchr(const char *s, int c);
int ft_strncmp(const char *s1, const char *s2, size_t n);
void *ft_memchr(const void *s, int c, size_t n);
int ft_memcmp(const void *s1, const void *s2, size_t n);
char *ft_strnstr(const char *haystack, const char *needle, size_t len);
int ft_atoi(const char *str);
void *ft_calloc(size_t count, size_t size);
char *ft_strdup(const char *s1);
char *ft_substr(char const *s, unsigned int start, size_t len);
char *ft_strjoin(char const *s1, char const *s2);
char *ft_strtrim(char const *s1, char const *set);
char **ft_split(char const *s, char c);
char *ft_itoa(int n);
char *ft_strmapi(char const *s, char (*f)(unsigned int, char));
void ft_striteri(char *s, void (*f)(unsigned int, char*));
void ft_putchar_fd(char c, int fd);
void ft_putstr_fd(char *s, int fd);
void ft_putendl_fd(char *s, int fd);
void ft_putnbr_fd(int n, int fd);
t_list *ft_lstnew(void *content);
void ft_lstadd_front(t_list **lst, t_list *new);
int ft_lstsize(t_list *lst);
t_list *ft_lstlast(t_list *lst);
void ft_lstadd_back(t_list **lst, t_list *new);
void ft_lstdelone(t_list *lst, void (*del)(void *));
void ft_lstclear(t_list **lst, void (*del)(void *));
void ft_lstiter(t_list *lst, void (*f)(void *));
t_list *ft_lstmap(t_list *lst, void *(*f)(void *), void (*del)(void *));
size_t ft_maximum_sizet(size_t n, ...);
size_t ft_minimum_sizet(size_t n, ...);
void ft_putnstr_fd(char *s, int fd, int n);

/*ft_printf functions*/
int ft_printf(const char *fmt, ...);
t_print *ft_spec_init(t_print *t_spec);
t_print *ft_spec_reset(t_print *t_spec);
void *flags_characters(const char *format, int i, t_print *t_spec);
int print_format(const char *format, int i, t_print *t_spec);
int check_conversion(const char *format, int i, t_print *t_spec);
int flag_precision(const char *format, int i, t_print *t_spec);
int flag_width(const char *format, int i, t_print *t_spec);
int print_char(int i, t_print *t_spec);
int print_str(int i, t_print *t_spec);
int print_percentage(int i, t_print *t_spec);
int print_d(int i, t_print *t_spec);
int print_unsigned(int i, t_print *t_spec);
int print_x(int i, t_print *t_spec);
int print_upperX(int i, t_print *t_spec);
int print_p(int i, t_print *t_spec);
int ft_print_zeros(int zeros);
int ft_print_spaces(int spaces);


/*libft code */

void ft_putchar_fd(char c, int fd)
{
 write(fd, &c, 1);
}

void ft_putendl_fd(char *s, int fd)
{
 int i;

 if (s == NULL)
 return ;
 i = 0;
 while (s[i] != '\0')
 {
 write(fd, &s[i], 1);
 i++;
 }
 write(fd, "\n", 1);
}

void ft_putnbr_fd(int n, int fd)
{
 int i;
 char d;

 if (n == -2147483648)
 {
 write(fd, "-2147483648", 11);
 return ;
 }
 i = 0;
 if (n < 0)
 {
 write(fd, "-", 1);
 n = n * (-1);
 }
 if (n <= 9)
 {
 d = '0' + n;
 write(fd, &d, 1);
 }
 else
 {
 ft_putnbr_fd(((n - (n % 10)) / 10), fd);
 d = '0' + (n % 10);
 write(fd, &d, 1);
 }
}

void ft_putnstr_fd(char *s, int fd, int n)
{
 int i;

 if (s == NULL)
 return ;
 i = 0;
 while ((s[i] != '\0') && (i < n))
 {
 write(fd, &s[i], 1);
 i++;
 }
}

void ft_putstr_fd(char *s, int fd)
{
 int i;

 if (s == NULL)
 return ;
 i = 0;
 while (s[i] != '\0')
 {
 write(fd, &s[i], 1);
 i++;
 }
}
size_t ft_strlen(const char *str)
{
 size_t n;

 n = 0;
 while (str[n] != '\0')
 n++;
 return (n);
}

char *ft_strdup(const char *s1)
{
 char *copy;

 copy = (char *)malloc(ft_strlen(s1) + 1);
 if (copy)
 ft_strlcpy(copy, s1, ft_strlen(s1) + 1);
 return (copy);
}

size_t ft_strlcpy(char *dst, const char *src, size_t dstsize)
{
 size_t i;
 char *csrc;

 csrc = (char *)src;
 i = 0;
 if (dstsize > 0)
 {
 while (i < dstsize - 1 && csrc[i])
 {
 dst[i] = csrc[i];
 i++;
 }
 dst[i] = '\0';
 }
 return (ft_strlen(csrc));
}

char *ft_substr(char const *s, unsigned int start, size_t len)
{
 char *sub;
 unsigned int i;

 if (s == NULL)
 return (NULL);
 else if (start <= (unsigned int)ft_strlen(s))
 {
 if (ft_strlen(s) < len)
 sub = (char *)malloc(ft_strlen(s) + 1);
 else
 sub = (char *)malloc(len + 1);
 if (sub == NULL)
 return (NULL);
 i = start;
 ft_strlcpy(sub, &s[i], len + 1);
 return (sub);
 }
 else
 return (ft_strdup(""));
}

char *ft_strrchr(const char *s, int c)
{
 int len;
 char *cs;

 cs = (char *)s;
 len = ft_strlen(cs);
 while (len >= 0)
 {
 if (cs[len] == (c % 256))
 return (&cs[len]);
 len--;
 }
 return (NULL);
}
char *ft_strchr(const char *s, int c)
{
 int i;
 char *cs;

 cs = (char *)s;
 i = 0;
 while (cs[i] != (char)c)
 {
 if (cs[i] == '\0')
 {
 return (NULL);
 }
 i++;
 }
 return (&cs[i]);
}

char *ft_strtrim(char const *s1, char const *set)
{
 char *trim;
 size_t start;
 size_t end;

 if (s1 == NULL)
 return (NULL);
 start = 0;
 end = ft_strlen(s1) - 1;
 while (ft_strchr(set, s1[start]) != NULL && start <= end)
 start++;
 if (s1[start] == '\0')
 return (ft_strdup(""));
 while (ft_strchr(set, s1[end]) != NULL && end > 0)
 end--;
 trim = ft_substr(s1, (unsigned int)start, (end + 1 - start));
 return (trim);
}




int ft_tolower(int c)
{
 int a;

 if (('A' <= c && c <= 'Z' ))
 {
 a = c + 32;
 return (a);
 }
 else
 return (c);
}

int ft_toupper(int c)
{
 int a;

 if (('a' <= c && c <= 'z' ))
 {
 a = c - 32;
 return (a);
 }
 else
 return (c);
}

static int ft_isspace(char c)
{
 if ((c == '\t' || c == '\v' || c == '\f' || c == '\r' || c == '\n'
 || c == ' '))
 return (1);
 return (0);
}

int ft_atoi(const char *str)
{
 int n;
 int i;
 int sign;

 n = 0;
 i = 0;
 sign = 1;
 while (ft_isspace(*str))
 str++;
 if (str[i] == '-')
 {
 sign = -1;
 i++;
 }
 else if (str[i] == '+')
 i++;
 while (str[i] >= '0' && str[i] <= '9')
 {
 n = n * 10 + (str[i] - '0');
 i++;
 }
 return (sign * n);
}

void ft_bzero(void *s, size_t n)
{
 ft_memset(s, 0, n);
}

void *ft_calloc(size_t count, size_t size)
{
 unsigned char *pr;

 pr = (unsigned char *)malloc(count * size);
 if (pr == NULL)
 return (NULL);
 ft_bzero(pr, (count * size));
 return ((void *)pr);
}

int ft_isalnum(int c)
{
 if ((('a' <= c && c <= 'z' ) || ('A' <= c && c <= 'Z' ))
 || ('0' <= c && c <= '9'))
 return (1);
 else
 return (0);
}

int ft_isalpha(int c)
{
 if ((c >= 'a' && c <= 'z' ) || (c >= 'A' && c <= 'Z' ))
 return (1);
 else
 return (0);
}

int ft_isascii(int c)
{
 if (c >= 0 && c <= 127)
 return (1);
 else
 return (0);
}

int ft_isdigit(int c)
{
 if (c >= '0' && c <= '9')
 return (1);
 else
 return (0);
}

int ft_isprint(int c)
{
 if (c >= 32 && c <= 126)
 return (1);
 else
 return (0);
}

static int len_int(int n)
{
 int len;
 int d;

 len = 1;
 d = 10;
 if (n == -2147483648)
 len = 11;
 else
 {
 if (n < 0)
 {
 n = n * (-1);
 len++;
 }
 while (d <= n)
 {
 len++;
 if (d == 1000000000)
 break ;
 d = d * 10;
 }
 }
 return (len);
}

char *ft_itoa(int n)
{
 char *str;
 int i;
 int len;

 i = 0;
 if (n == -2147483648)
 return (ft_strdup("-2147483648"));
 len = len_int(n);
 str = (char *)malloc(len + 1);
 if (str == NULL)
 return (NULL);
 if (n < 0)
 {
 str[i] = '-';
 i++;
 n = n * (-1);
 }
 str[len] = '\0';
 while ((len - 1) >= i)
 {
 str [len - 1] = '0' + (n % 10);
 n = (n - (n % 10)) / 10;
 len--;
 }
 return (str);
}

void ft_lstadd_back(t_list **lst, t_list *new)
{
 t_list *last;

 if (new == NULL)
 return ;
 if (*lst == NULL)
 {
 *lst = new;
 new->next = NULL;
 return ;
 }
 last = ft_lstlast(*lst);
 last->next = new;
}

void ft_lstadd_front(t_list **lst, t_list *new)
{
 if (new != NULL)
 {
 new->next = *lst;
 *lst = new;
 }
}

void ft_lstclear(t_list **lst, void (*del)(void *))
{
 t_list *head;
 t_list *temp;

 temp = *lst;
 if (*lst == NULL)
 return ;
 while (temp != NULL)
 {
 head = temp->next;
 del(temp->content);
 free (temp);
 temp = head;
 }
 *lst = NULL;
}

void ft_lstdelone(t_list *lst, void (*del)(void *))
{
 del(lst->content);
 free(lst);
}

void ft_lstiter(t_list *lst, void (*f)(void *))
{
 while (lst != NULL)
 {
 f(lst->content);
 lst = lst->next;
 }
}

t_list *ft_lstlast(t_list *lst)
{
 while ((lst != NULL) && (lst->next != NULL))
 lst = lst->next;
 return (lst);
}

t_list *ft_lstnew(void *content)
{
 t_list *newelement;

 newelement = (t_list *)malloc(sizeof(t_list));
 if (newelement == NULL)
 return (NULL);
 newelement->content = content;
 newelement->next = NULL;
 return (newelement);
}

t_list *ft_lstmap(t_list *lst, void *(*f)(void *), void (*del)(void *))
{
 t_list *list;
 t_list *new;

 if ((lst == NULL || f == NULL))
 return (NULL);
 list = NULL;
 while (lst != NULL)
 {
 new = ft_lstnew((*f)(lst->content));
 if (new == NULL)
 {
 ft_lstclear(&list, del);
 return (NULL);
 }
 ft_lstadd_back(&list, new);
 lst = lst->next;
 }
 return (list);
}

int ft_lstsize(t_list *lst)
{
 int size;

 size = 0;
 while (lst != NULL)
 {
 lst = lst->next;
 size++;
 }
 return (size);
}

size_t ft_maximum_sizet(size_t n, ...)
{
 size_t i;
 size_t val;
 size_t largest;
 va_list vl;

 va_start (vl, n);
 largest = va_arg(vl, size_t);
 i = 1;
 while (i < n)
 {
 val = va_arg(vl, size_t);
 if (largest < val)
 largest = val;
 i++;
 }
 va_end(vl);
 return (largest);
}

void *ft_memchr(const void *s, int c, size_t n)
{
 unsigned char *us;
 size_t i;

 us = (unsigned char *)s;
 i = 0;
 while (i < n)
 {
 if (us[i] == (unsigned char)c)
 {
 return ((void *)&us[i]);
 }
 i++;
 }
 return (NULL);
}

int ft_memcmp(const void *s1, const void *s2, size_t n)
{
 size_t i;
 unsigned char *us1;
 unsigned char *us2;

 us1 = (unsigned char *)s1;
 us2 = (unsigned char *)s2;
 i = 0;
 while (i < n)
 {
 if (us1[i] < us2[i])
 return (us1[i] - us2[i]);
 else if (us1[i] > us2[i])
 return (us1[i] - us2[i]);
 else
 i++;
 }
 return (0);
}

void *ft_memcpy(void *dst, const void *src, size_t n)
{
 unsigned char *cdst;
 unsigned char *csrc;
 size_t i;

 if (!dst && !src)
 return (NULL);
 cdst = (unsigned char *)dst;
 csrc = (unsigned char *)src;
 i = 0;
 while (i < n)
 {
 cdst[i] = csrc[i];
 i++;
 }
 return (dst);
}

void *ft_memmove(void *dst, const void *src, size_t len)
{
 char *cdst;
 char *csrc;
 size_t i;

 if (!dst && !src)
 return (NULL);
 csrc = (char *)src;
 cdst = (char *)dst;
 i = 0;
 if (csrc > cdst)
 {
 while (i < len)
 {
 *(cdst + i) = *(csrc + i);
 i++;
 }
 return (cdst);
 }
 while (len)
 {
 len--;
 *(cdst + len) = *(csrc + len);
 }
 return (cdst);
}

void *ft_memset(void *b, int c, size_t len)
{
 size_t i;
 unsigned char *a;

 i = 0;
 a = (unsigned char *)b;
 while (i < len)
 {
 a[i] = (unsigned char)c;
 i ++;
 }
 return (b);
}

size_t ft_minimum_sizet(size_t n, ...)
{
 size_t i;
 size_t val;
 size_t minimum;
 va_list vl;

 va_start (vl, n);
 minimum = va_arg(vl, size_t);
 i = 1;
 while (i < n)
 {
 val = va_arg(vl, size_t);
 if (minimum > val)
 minimum = val;
 i++;
 }
 va_end(vl);
 return (minimum);
}

static int countwords(const char *s, int c)
{
 int words;
 int i;

 i = 0;
 words = 0;
 while (s[i] != '\0' && s[i] == c)
 {
 i++;
 }
 if (s[i] != '\0')
 words = 1;
 while (s[i] != '\0')
 {
 if (s[i] == c)
 {
 if ((s[i + 1] != '\0') && (s[i + 1] != c))
 words++;
 }
 i++;
 }
 return (words);
}

static char **clean(char **split, int n)
{
 int i;

 i = 0;
 while (i <= n)
 {
 free (split[i]);
 i++;
 }
 free(split);
 return (NULL);
}

static char **newarray(char const *s, char **split, char c)
{
 int i;
 int k;
 int start;
 int end;

 i = 0;
 k = 0;
 while (s[k] == c)
 k++;
 while (s[k] != '\0')
 {
 start = k;
 while (s[k] != c && s[k] != '\0')
 k++;
 end = k;
 split[i] = ft_substr(s, start, end - start);
 if (split[i] == NULL)
 clean(split, i);
 i++;
 while (s[k] == c)
 k++;
 }
 split[i] = NULL;
 return (split);
}

static char **onewordarray(char const *s, char **split, char c)
{
 char *set;

 set = &c;
 split[0] = ft_strtrim(s, set);
 if (split[0] == NULL)
 clean(split, 0);
 split[1] = NULL;
 return (split);
}

char **ft_split(char const *s, char c)
{
 int words;
 char **split;

 if (s == NULL)
 return (NULL);
 words = countwords(s, c);
 split = (char **)malloc(sizeof(char *) * (words + 1));
 if (split == NULL)
 return (NULL);
 if (words == 0)
 {
 split[0] = NULL;
 return (split);
 }
 if (words == 1)
 {
 return (onewordarray(s, split, c));
 }
 else
 return (newarray(s, split, c));
}

t_print *ft_spec_init(t_print *t_spec)
{
 t_spec->number = false;
 t_spec->zero = false;
 t_spec->dash = false;
 t_spec->space = false;
 t_spec->sign = false;
 t_spec->width = false;
 t_spec->width_details = 0;
 t_spec->precision = false;
 t_spec->precision_details = 0;
 t_spec->len_total = 0;
 return (t_spec);
}

t_print *ft_spec_reset(t_print *t_spec)
{
 t_spec->number = false;
 t_spec->zero = false;
 t_spec->dash = false;
 t_spec->space = false;
 t_spec->sign = false;
 t_spec->width = false;
 t_spec->width_details = 0;
 t_spec->precision = false;
 t_spec->precision_details = 0;
 return (t_spec);
}

int print_format(const char *format, int i, t_print *t_spec)
{
 int count;

 count = 0;
 while (format[i] != '\0')
 {
 if (format[i] == '%')
 {
 i++;
 flags_characters(format, i, t_spec);
 while (ft_strchr("csdiupxX%", format[i]) == NULL)
 i++;
 i = check_conversion(format, i, t_spec);
 ft_spec_reset(t_spec);
 }
 else
 {
 count += write(1, &format[i], 1);
 i++;
 }
 }
 return (count);
}

void *flags_characters(const char *format, int i, t_print *t_spec)
{
 while (ft_strchr("csdiupxX%", format[i]) == NULL)
 {
 if (format[i] == '#')
 t_spec->number = true;
 if (format[i] == '0')
 t_spec->zero = true;
 if (format[i] == '-')
 t_spec->dash = true;
 if (format[i] == ' ')
 t_spec->space = true;
 if (format[i] == '+')
 t_spec->sign = true;
 if (ft_isdigit(format[i]) || (format[i] == '*'))
 i = flag_width(format, i, t_spec);
 if (format[i] == '.')
 i = flag_precision(format, i + 1, t_spec);
 i++;
 }
 return (t_spec);
}

static size_t width_details(t_print *t_spec)
{
 int width;

 width = va_arg((t_spec->args), int);
 if (width < 0)
 {
 width = width * (-1);
 t_spec->dash = true;
 t_spec->zero = false;
 }
 return ((size_t)width);
}

int flag_width(const char *format, int i, t_print *t_spec)
{
 size_t width;

 width = 0;
 t_spec->width = true;
 if (format[i] == '*')
 {
 t_spec->width_details = width_details(t_spec);
 i++;
 }
 if (ft_isdigit(format[i]))
 {
 while (ft_isdigit(format[i]))
 {
 width = width * 10 + (format[i] - '0');
 i++;
 }
 t_spec->width_details = width;
 }
 return (i - 1);
}

static size_t precision_details(t_print *t_spec)
{
 int precision;

 precision = va_arg((t_spec->args), int);
 if (precision < 0)
 {
 precision = 0;
 t_spec->precision = false;
 }
 return ((size_t)precision);
}

int flag_precision(const char *format, int i, t_print *t_spec)
{
 size_t precision;

 t_spec->precision = true;
 precision = 0;
 if (format[i] == '*')
 {
 t_spec->precision_details = precision_details(t_spec);
 i++;
 }
 else
 {
 while (ft_isdigit(format[i]))
 {
 precision = precision * 10 + (format[i] - '0');
 i++;
 }
 t_spec->precision_details = precision;
 }
 return (i - 1);
}

int ft_print_spaces(int spaces)
{
 int j;

 j = 0;
 while (j < spaces)
 {
 ft_putchar_fd(' ', 1);
 j++;
 }
 return (spaces);
}

int check_conversion(const char *format, int i, t_print *t_spec)
{
 if (format[i] == 'd' || format[i] == 'i')
 i = print_d(i, t_spec);
 else if (format[i] == 'c')
 i = print_char(i, t_spec);
 else if (format[i] == 's')
 i = print_str(i, t_spec);
 else if (format[i] == '%')
 i = print_percentage(i, t_spec);
 else if (format[i] == 'u')
 i = print_unsigned(i, t_spec);
 else if (format[i] == 'x')
 i = print_x(i, t_spec);
 else if (format[i] == 'X')
 i = print_upperX(i, t_spec);
 else if (format[i] == 'p')
 i = print_p(i, t_spec);
 return (i);
}


int ft_print_str_width(char *str, int len, t_print *t_spec)
{
 if (!(t_spec->dash) && !(t_spec->zero))
 {
 ft_print_spaces(t_spec->width_details - len);
 ft_putnstr_fd(str, 1, len);
 }
 if (t_spec->dash)
 {
 ft_putnstr_fd(str, 1, len);
 ft_print_spaces(t_spec->width_details - len);
 }
 if (!(t_spec->dash) && (t_spec->zero))
 {
 ft_print_zeros(t_spec->width_details - len);
 ft_putnstr_fd(str, 1, len);
 }
 return (t_spec->width_details);
}

int print_char(int i, t_print *t_spec)
{
 char c;

 c = (char) va_arg(t_spec->args, int);
 if (t_spec->width && !(t_spec->dash))
 ft_print_spaces(t_spec->width_details - 1);
 ft_putchar_fd(c, 1);
 if (t_spec->width && t_spec->dash)
 ft_print_spaces(t_spec->width_details - 1);
 t_spec->len_total = t_spec->len_total
 + ft_maximum_sizet(2, 1, t_spec->width_details);
 return (i + 1);
}

int print_percentage(int i, t_print *t_spec)
{
 char c;

 c = '%';
 if (t_spec->width && !(t_spec->dash))
 {
 if (t_spec->zero == true)
 ft_print_zeros(t_spec->width_details - 1);
 else
 ft_print_spaces(t_spec->width_details - 1);
 }
 ft_putchar_fd(c, 1);
 if (t_spec->width && t_spec->dash)
 ft_print_spaces(t_spec->width_details - 1);
 t_spec->len_total = t_spec->len_total
 + ft_maximum_sizet(2, 1, t_spec->width_details);
 return (i + 1);
}

static size_t print_null(t_print *t_spec)
{
 size_t len;

 if (!(t_spec->precision))
 len = 6;
 else
 len = ft_minimum_sizet(2, 6, t_spec->precision_details);
 if (t_spec->width_details > len)
 len = ft_print_str_width("(null)", len, t_spec);
 else
 ft_putnstr_fd("(null)", 1, len);
 t_spec->len_total = t_spec->len_total + len;
 return (len);
}

int print_str(int i, t_print *t_spec)
{
 char *str;
 size_t len;

 str = va_arg(t_spec->args, char *);
 if (!str)
 len = print_null(t_spec);
 else
 {
 if (!(t_spec->precision))
 len = ft_strlen(str);
 else
 len = ft_minimum_sizet(2, ft_strlen(str),
 t_spec->precision_details);
 if (t_spec->width_details > len)
 len = ft_print_str_width(str, len, t_spec);
 else
 ft_putnstr_fd(str, 1, len);
 t_spec->len_total = t_spec->len_total + len;
 }
 return (i + 1);
}

static size_t ft_len_int(int n)
{
 size_t digits;

 digits = 1;
 while (n / 10)
 {
 digits++;
 n /= 10;
 }
 return (digits);
}

static size_t print_prec(int num, size_t len, size_t counter, t_print *t_spec)
{
 if (t_spec->precision_details > len)
 {
 if (num == -2147483648)
 write(1, "-", 1);
 ft_print_zeros(t_spec->precision_details - len);
 counter += (t_spec->precision_details - len);
 }
 if ((num == -2147483648) && (t_spec->precision_details > len))
 write(1, "2147483648", 10);
 else
 ft_putnbr_fd(num, 1);
 return (counter);
}

static size_t print_sign(int num, size_t len, t_print *t_spec)
{
 size_t counter;

 counter = 0;
 if (num == -2147483648)
 counter ++;
 else if ((t_spec->sign == true) && (num >= 0))
 counter += write(1, "+", 1);
 else if (num < 0)
 {
 counter += write(1, "-", 1);
 num = num * (-1);
 }
 else if (t_spec->space == true)
 counter += write(1, " ", 1);
 if (len > 0)
 counter = print_prec(num, len, counter, t_spec);
 return (len + counter);
}

static size_t print_width(int num, size_t len, size_t max, t_print *t_spec)
{
 if (!t_spec->dash)
 {
 if ((t_spec->zero == true) && (t_spec->precision == false))
 {
 if (num < 0)
 t_spec->precision_details = t_spec->width_details - 1;
 else
 t_spec->precision_details = t_spec->width_details;
 }
 else
 ft_print_spaces(t_spec->width_details - max);
 len = print_sign(num, len, t_spec);
 }
 else
 {
 len = print_sign(num, len, t_spec);
 //if ((t_spec->zero == true) && (t_spec->precision == false))
 // ft_print_zeros(t_spec->width_details - max);
 //else
 ft_print_spaces(t_spec->width_details - max);
 }
 return (t_spec->width_details);
}
/* Line# 100. If the number is 0(lenght of the string) and && the precision is .0 then we donot have to print anything */ 

int print_d(int i, t_print *t_spec)
{
 int num;
 size_t len;
 size_t max;

 num = va_arg(t_spec->args, int);
 len = ft_len_int(num);
 if ((num == 0) && (t_spec->precision) && (t_spec->precision_details == 0))
 len = 0;
 max = ft_maximum_sizet(2, len, t_spec->precision_details);
 if ((num < 0) || (t_spec->sign == true) || (t_spec->space == true))
 max++;
 if (!t_spec->precision)
 t_spec->precision_details = 1;
 if (t_spec->width_details > max)
 len = print_width(num, len, max, t_spec);
 else
 len = print_sign(num, len, t_spec);
 t_spec->len_total += len;
 return (i + 1);
}

static size_t len_unum(unsigned long int n)
{
 size_t digits;

 digits = 1;
 while (n / 16)
 {
 digits++;
 n /= 16;
 }
 return (digits);
}

static void ft_putnum_base(unsigned long int n, int fd, char *base)
{
 if (n / 16)
 ft_putnum_base(n / 16, fd, base);
 ft_putchar_fd(base[n % 16], fd);
}

static size_t print_numu_sign(unsigned long int n, size_t l, t_print *t_spec)
{
 size_t count;

 count = 0;
 count += write(1, "0x", 2);
 if (l > 0)
 {
 if (t_spec->precision_details > l)
 {
 ft_print_zeros(t_spec->precision_details - l);
 count += (t_spec->precision_details - l);
 }
 }
 ft_putnum_base(n, 1, "0123456789abcdef");
 return (l + count);
}

static size_t p_w(unsigned long int n, size_t l, size_t max, t_print *t_spec)
{
 if (!t_spec->dash)
 {
 if ((t_spec->zero == true) && (t_spec->precision == false))
 t_spec->precision_details = t_spec->width_details;
 else
 ft_print_spaces(t_spec->width_details - max);
 l = print_numu_sign(n, l, t_spec);
 }
 else
 {
 l = print_numu_sign(n, l, t_spec);
 if ((t_spec->zero == true) && (t_spec->precision == false))
 ft_print_zeros(t_spec->width_details - max);
 else
 ft_print_spaces(t_spec->width_details - max);
 }
 return (t_spec->width_details);
}

int print_p(int i, t_print *t_spec)
{
 unsigned long int num;
 size_t len;
 size_t max;

 num = (unsigned long int)va_arg(t_spec->args, void *);
 len = len_unum(num);
 max = ft_maximum_sizet(2, len, t_spec->precision_details) + 2;
 if (!t_spec->precision)
 t_spec->precision_details = 1;
 if (t_spec->width_details > max)
 len = p_w(num, len, max, t_spec);
 else
 len = print_numu_sign(num, len, t_spec);
 t_spec->len_total += len;
 return (i + 1);
}

int print_unsigned(int i, t_print *t_spec)
{
 unsigned int num;
 size_t len;
 size_t max;

 num = va_arg(t_spec->args, unsigned int);
 len = len_unum(num);
 if ((num == 0) && (t_spec->precision) && (t_spec->precision_details == 0))
 len = 0;
 max = ft_maximum_sizet(2, len, t_spec->precision_details);
 if ((t_spec->sign == true) || (t_spec->space == true))
 max++;
 if (!t_spec->precision)
 t_spec->precision_details = 1;
 if (t_spec->width_details > max)
 len = p_w(num, len, max, t_spec);
 else
 len = print_numu_sign(num, len, t_spec);
 t_spec->len_total += len;
 return (i + 1);
}

int print_upperX(int i, t_print *t_spec)
{
 unsigned int num;
 size_t len;
 size_t max;

 num = va_arg(t_spec->args, unsigned int);
 len = len_unum(num);
 if ((num == 0) && (t_spec->precision) && (t_spec->precision_details == 0))
 len = 0;
 max = ft_maximum_sizet(2, len, t_spec->precision_details);
 if ((t_spec->number) && (num != 0))
 max = max + 2;
 if (!t_spec->precision)
 t_spec->precision_details = 1;
 if (t_spec->width_details > max)
 len = p_w(num, len, max, t_spec);
 else
 len = print_numu_sign(num, len, t_spec);
 t_spec->len_total += len;
 return (i + 1);
}

int print_x(int i, t_print *t_spec)
{
 unsigned int num;
 size_t len;
 size_t max;

 num = va_arg(t_spec->args, unsigned int);
 len = len_unum(num);
 if ((num == 0) && (t_spec->precision) && (t_spec->precision_details == 0))
 len = 0;
 max = ft_maximum_sizet(2, len, t_spec->precision_details);
 if ((t_spec->number) && (num != 0))
 max = max + 2;
 if (!t_spec->precision)
 t_spec->precision_details = 1;
 if (t_spec->width_details > max)
 len = p_w(num, len, max, t_spec);
 else
 len = print_numu_sign(num, len, t_spec);
 t_spec->len_total += len;
 return (i + 1);
}

int ft_print_zeros(int zeros)
{
 int j;

 j = 0;
 while (j < zeros)
 {
 ft_putchar_fd('0', 1);
 j++;
 }
 return (zeros);
}


int ft_printf(const char *format, ...)
{
 int i;
 int count;
 t_print *t_spec;

 t_spec = (t_print *)malloc(sizeof(t_print));
 if (!t_spec)
 return (-1);
 ft_spec_init(t_spec);
 va_start(t_spec->args, format);
 i = 0;
 count = print_format(format, i, t_spec);
 count += t_spec->len_total;
 va_end(t_spec->args);
 free(t_spec);
 return (count);
}

int main(void)
{

    ft_printf("%d", 42);
    return (0);
}

```
