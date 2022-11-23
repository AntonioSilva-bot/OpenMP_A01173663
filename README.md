# OpenMP_A01173663
Programa en C usando OpenMP que realiza el efecto espejo con 20 diferentes imagenes de más de 2000 pixeles por lado.

En este código lo que se hace es invertir imagenes bmp, este tambien cuenta con la capacidad de poder poner a escalas de grises o en su color la imagen invertida, este código funciona en el lenguaje c y las imagenes utilizadas son de un tamaño superior a 2000 pixeles.

Para poder conseguir este efecto necesitamos utilizar las siguientes librerías:

```cpp
#include <stdio.h>
#include <stdlib.h>
```

En la siguiente parte definimos la cantidad de threads y las variables globales que utizaremos dentro del código.

```cpp
#define NUM_THREADS 6
struct my_pixel{
  unsigned char r, g, b;
};
```
En esta parte del código abrimos las imagenes para poder realizar el espejeo, siendo que en este caso se ha optado por utilizar una función void para poder realizar esto. Dentro de esta función guardamos los valores de las medidas de cada imagen, tambien se declara el nombre de salida del archivo y que nombre tendrá

```cpp
   char *imagenes[]={"nombre_de_imagen"};
    char *nuevas[]={"nombre_de_imagen_resultante"};
    int i, tam= sizeof(imagenes)/sizeof(char *);
    for(int i=0; i<tam;++i){
        FILE *image, *outputImage, *lecturas;
        image = fopen(imagenes[i],"rb");          //Imagen original a transformar
        outputImage = fopen(nuevas[i],"wb");    //Imagen transformada
        long ancho;
        long alto;
        unsigned char r, g, b;               //Pixel
```
Con los valores obtenidos podemos conocer y proyectar las medidas de cada imagen, además estas nos servirán para hacer el espejeo de la imagen
```cpp
        unsigned char xx[54];
        for(int i=0; i<54; i++) {
        xx[i] = fgetc(image);
        fputc(xx[i], outputImage); 
        }
        ancho = (long)xx[20]*65536+(long)xx[19]*256+(long)xx[18];
        alto = (long)xx[24]*65536+(long)xx[23]*256+(long)xx[22];
        printf("largo img %li\n",alto);
        printf("ancho img %li\n",ancho);

```
A continuación se utiliza el padding para poder hacer que la imagen corresponda a una bmp, ya que lo que hace la siguiente parte del código es rellenar los espacios en blanco y así evitar un desplazamiento de esta 
```cpp
padding = ancho%4;
        printf("%d\n", j);
        int count = 0;
        //omp_set_num_threads(4);
        #pragma omp parallel for schedule(dynamic 3) 
        for(int i = 0; i < ancho*alto*3; i+=3){

                b = arr_in [i];
                g = arr_in [i + 1];
                r = arr_in [i + 2];
                unsigned char pixel = 0.21*r+0.72*g+0.07*b;
                // arr_in  [i] = pixel;
                // arr_in  [i + 1] = pixel;
                // arr_in  [i + 2] = pixel;
                arr_in  [i] = b;
                arr_in  [i + 1] = g;
                arr_in  [i + 2] = r;
                count += 3;
                if(count == ancho*3){
                    i+=padding;
                    count = 0;
                }
        for(int i = 0; i < alto; i++){
            for(int j = 0; j < (ancho*3); j+= 3){
                arr_out [(i*((ancho*3)+padding))+j-3] = arr_in  [(i*((ancho*3)+padding))+ (ancho*3) - j];
                arr_out [(i*((ancho*3)+padding))+j-4]  = arr_in  [(i*((ancho*3)+padding))+ (ancho*3 )- j - 1];
                arr_out [(i*((ancho*3)+padding))+j-5]  = arr_in  [(i*((ancho*3)+padding))+ (ancho*3) - j - 2];
            }
        }

        for(int i = 0; i < ancho*alto*3; i++){
            fputc(arr_out [i], outputImage);

```

Por ultimo se crea el archivo de salida y el proceso anteriormente realizado es llamado por el main

```cpp
        fclose(image);
        fclose(outputImage);

    }
    printf("Termine\n"); 
}
int main()
{
    imagemirror();
    return 0;
}
'''
