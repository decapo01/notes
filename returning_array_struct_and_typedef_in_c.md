

```c

#include <stdio.h>
#include <string.h>
#include <stdlib.h>

char global_arr [100];

char *func(){
  global_arr[0] = 'h';
  global_arr[1] = 'e';
  return global_arr;
}


char *func_malloc(){

  char *buffer = malloc(100);

  buffer[0] = 'h';
  buffer[1] = 'e';
  buffer[2] = 'l';

  return buffer;
}


void func_void(char *result, int size){

  strncpy(result, "Hello World", size);
}

const char *items = "hello_world";


# define SIZE 100

//const int SIZE = 100;

typedef struct Data {
  char buffer[SIZE];
} Data;

Data func_data(){
  Data d;
  strncpy(d.buffer, "hello world form data", SIZE);
  return d;
}

int main(void) {

  //char *malloc_items = func_malloc();

  char *buffer = malloc(11);

  func_void(buffer,11);

  printf("%s\n",buffer);

  free(buffer);

  Data data = func_data();

  printf("%s\n",data.buffer);

  return 0;
}
```