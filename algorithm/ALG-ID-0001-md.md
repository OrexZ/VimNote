---
title: 巧字决 - [ALG-ID-0001]
permalink: algorithm-ALG-ID-0001
date: 2019-09-02 12:58:50
categories:
tags:
---

## 问题描述

将字符数组中的'空格'符号转换成'％20',字符数组长度足够，可将结果输出到原数组或者新数组。

## 解题方法

实现方法一：

```C
#include <stdio.h>
#include <string.h>

int count_of_space(char * in){
  int i, num_space=0;
  for (i=0; i<strlen(in); i++){
    if (in[i] == ' '){
      num_space += 1;
    }
  }
  return num_space;
}

#define REPLACE_LEN 2
#define REPLACE_ITEM "%20"
int fill_space(char * str, int pos){
  int i;
  char * c = REPLACE_ITEM;

  for (i=0; i<strlen(REPLACE_ITEM); i++){
    str[pos--] = c[strlen(REPLACE_ITEM) - i - 1];
  }

  return REPLACE_LEN;
}

#define POS(current_pos) (current_pos - 1)
int replace_space(char * in, char * out, unsigned size){
  int i, len =strlen(in);
  int num_space = count_of_space(in);
  int len_with_spec = len + num_space * 2;
  int iterc = 0;

  if (len + 1 >= size || len_with_spec + 1 > size){
    return -1;
  }

  for (i=0; i<len; i++){
    if (in[POS(len-i)] == ' '){
      iterc += fill_space(out, POS(len_with_spec-i) - iterc);
    }
    else {
      out[POS(len_with_spec-i) - iterc] = in[POS(len-i)];
    }
  }

  out[len_with_spec + 1] = '\0';

  return 0;
}

int main(void){

#define BUF_SIZE 100
  char buf[BUF_SIZE] = "Hello, My name is [wakaka] !";

  printf("\n[IN STRING] %s\n", buf);

  if (replace_space(buf, buf, BUF_SIZE) != 0){
    printf("Replace space FAILD.\n");
  }

  printf("\n[OUT STRING] %s\n", buf);

  return 0;
}
```

不使用标准库的实现二：

```C
#include <stdio.h>

unsigned get_char_num(char *in, char ops){
  unsigned i = 0, len = 0;
  if (ops == '\0'){
    while (*(in+i) != '\0'){
      i++;
      len++;
    }
  }else{
    while (*(in+i) != '\0'){
      if (*(in + i) == ops){
        len++;
      }
      i++;
    }
  }
  return len;
}

#define WANTED_STR "%20"
#define POS(len) (len - 1)

int replace_space(char *in, char *out, unsigned size){
  int i, num_space = 0;
  int len = 0, len_with_space = 0;
  int wanted_str_len = 0;

  len = get_char_num(in, '\0');
  num_space = get_char_num(in, ' ');
  wanted_str_len = get_char_num(WANTED_STR, '\0');
  len_with_space = len + num_space * 2;

  if (size < len_with_space + 1)
    return -1;

  out[len_with_space + 1] = '\0';

  while (len > 0){
    if (in[POS(len)] == ' '){
      for (i=0; i< wanted_str_len; i++){
        out[POS(len_with_space--)] = WANTED_STR[POS(wanted_str_len)-i];
      }
    }else{
      out[POS(len_with_space--)] = in[POS(len)];
    }
    len--;
  }
  return 0;
}

int main(void){
#define BUF_SIZE 100
  char buf[BUF_SIZE] = "Darling, I am so hoooooot .. .. !";

  printf("IN STRING: [%s]\n", buf);
  if (replace_space(buf, buf, BUF_SIZE) != 0)
    printf("Replace space FAIL.");
  printf("OUT STRING: [%s]\n", buf);

  return 0;
}
```

