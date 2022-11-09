# OpenMP_A01173663
Programa en C usando OpenMP que realiza el efecto espejo con 20 diferentes imagenes de más de 2000 pixeles por lado.

Este código tiene la funcionalidad de poder invertir una imagen bmp expejeandola y cambiando los colores a una escala de grises, para esto se ha utilizado 


```cpp
#include <stdio.h>
#include <stdlib.h>
#include <omp.h>

void img_create(){
  char org[16]; snprintf(org, 16, "input_%i.bmp", omp_get_thread_num()+1);
  char dst[16]; snprintf(dst, 16, "output_%i.bmp", omp_get_thread_num()+1);

  FILE *fptr_r; fptr_r=fopen(org,"rb");
  FILE *fptr_w; fptr_w=fopen(dst,"wb");
```
