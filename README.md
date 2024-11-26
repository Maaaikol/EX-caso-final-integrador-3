# Ejercicio de Programación.

<details>
  <summary>ENUNCIADO DE LOS EJERCICIOS: </summary>
  <p style="font-size: 12px; line-height: 1.4;">
<details>
  <summary>TIT.enun1</summary>
  <p style="font-size: 12px; line-height: 1.4;">

ENUNCIADO

</p>

</details>
<details>
  <summary>TIT.enun2</summary>
  <p style="font-size: 12px; line-height: 1.4;">

ENUNCIADO

</p>

</details>
<details>
  <summary>TIT.enun3</summary>
  <p style="font-size: 12px; line-height: 1.4;">

ENUNCIADO

</p>

</details>
<details>
  <summary>TIT.enun4</summary>
  <p style="font-size: 12px; line-height: 1.4;">

ENUNCIADO

</p>

</details>

<details>
  <summary>TIT.enun5</summary>
  <p style="font-size: 12px; line-height: 1.4;">

ENUNCIADO

</p>

</details>
</details>
<details>
  <summary>PROPUESTA DE SOLUCIÓN: </summary>
  <p style="font-size: 12px; line-height: 1.4;">

  ````
1. 
````

  ````
2. 
````

  ````
3. 
````

  ````
4. 
````

  ````
5. 
````

</p>
</details>

<details>
  <summary>LINK DEL REPOSITORIO:</summary>
  <p style="font-size: 12px; line-height: 1.4;">

  [Repositorio GitHub](https://github.com/Maaaikol/ESTRUCTURA-README-EJ.git)

</p>

</details>

<details>
  <summary> Corrección de Gonzalo Müller a Michael Rea </summary>

# Análisis de Problemas y Soluciones en el Código

*Potencial Desbordamiento de Buffer en la Entrada del Nombre de Archivo*

*Descripción del Problema:*

Usar scanf("%499s", filename); para leer el nombre del archivo proporcionado por el usuario plantea un riesgo de desbordamiento de buffer si el usuario ingresa más de 499 caracteres. Esto puede conducir a comportamientos indefinidos o corrupción de memoria.

*Propuesta de Solución:*

Para mitigar este riesgo, se recomienda utilizar una función que limite la cantidad de caracteres leídos y maneje adecuadamente los posibles excedentes. Una opción es utilizar fgets(filename, sizeof(filename), stdin);, que lee una línea completa de la entrada estándar y asegura que no se exceda el tamaño del buffer. Además, manejar el posible carácter de nueva línea \n al final de la entrada.

c 
if (fgets(filename, sizeof(filename), stdin) == NULL) { fprintf(stderr, "Error al leer el nombre del archivo\n"); return; } // Eliminar el carácter de nueva línea si está presente size_t len = strlen(filename); if (len > 0 && filename[len - 1] == '\n') { filename[len - 1] = '\0'; }


*Acceso Fuera de Límites en el Buffer 'script'*

*Descripción del Problema:*

El buffer script está definido con un tamaño de BUF_SIZE + 1 (4001 bytes). Asignar script[n] = '\0'; después de leer datos con fread podría causar un acceso fuera de límites si n es igual a BUF_SIZE.

*Propuesta de Solución:*

Asegurarse de que la función fread no lea más bytes de los que el buffer puede contener. Dado que se utiliza fread(script, 1, BUF_SIZE, f);, se limita correctamente el número de bytes leídos. El buffer script tiene un byte adicional para acomodar el terminador nulo, por lo que asignar script[n] = '\0'; es seguro siempre que n no exceda BUF_SIZE.

*Colores de Terminal No Restablecidos Después de Mostrar el Script*

*Descripción del Problema:*

Al utilizar códigos de escape ANSI como \033[34m y \033[47m para cambiar el color del texto y el fondo, si no se restablecen los colores después de imprimir, la salida posterior en la consola continuará con estos colores, lo cual puede no ser deseable.

*Propuesta de Solución:*

Agregar un código de escape ANSI para restablecer los colores del terminal a sus valores predeterminados después de imprimir el script. Esto se logra agregando \033[0m al final de la cadena formateada:

c
printf("\033[34m\033[47m%s\033[0m\n", script);


*Posible Fuga de Recursos si Ocurre un Error al Leer el Archivo*

*Descripción del Problema:*

Si ocurre un error durante la lectura del archivo y no se maneja adecuadamente, el archivo puede no cerrarse correctamente, lo que conduce a una fuga de recursos.

*Propuesta de Solución:*

Asegurar que el archivo se cierra en todas las rutas de ejecución, incluso si ocurre un error. Una forma de lograrlo es mover la llamada a fclose(f); después de la comprobación de errores, o utilizar una sección de limpieza para manejar la liberación de recursos:

c
FILE* f = fopen(filename, "rb"); if (!f) { fprintf(stderr, "Error al abrir %s\n", filename); return; }

// Código de lectura...

if (ferror(f)) { fprintf(stderr, "Error durante la lectura del archivo\n"); }

fclose(f);


*Funciones Duplicadas y Conflicto de Nombres*

*Descripción del Problema:*

Existen dos funciones llamadas load_script, una que acepta parámetros y otra que no. En C, esto no está permitido y puede causar errores de compilación debido a la redefinición de funciones.

*Propuesta de Solución:*

Renombrar la función sin parámetros a un nombre más descriptivo para evitar conflictos y mejorar la legibilidad del código. Por ejemplo, cambiarla a load_script_from_user_input:

c 
void load_script_from_user_input() { // Implementación... }


Actualizar las declaraciones en el archivo de cabecera y las llamadas a esta función en el código principal.

*Uso de 'scanf' Puede No Leer Entradas con Espacios*

*Descripción del Problema:*

La función scanf("%499s", filename); detiene la lectura al encontrar un carácter de espacio, lo que impide leer nombres de archivos que contengan espacios.

*Propuesta de Solución:*

Utilizar fgets para leer la línea completa de entrada, permitiendo nombres de archivo con espacios:

c 
if (fgets(filename, sizeof(filename), stdin) == NULL) { fprintf(stderr, "Error al leer el nombre del archivo\n"); return; } // Eliminar el carácter de nueva línea si está presente size_t len = strlen(filename); if (len > 0 && filename[len - 1] == '\n') { filename[len - 1] = '\0'; }


*Falta de Validación de Entrada para la Lectura Exitosa*

*Descripción del Problema:*

No se verifica si scanf o fgets logran leer correctamente la entrada del usuario, lo que podría resultar en el uso de datos no inicializados.

*Propuesta de Solución:*

Comprobar el valor de retorno de la función de entrada para asegurar que la lectura fue exitosa:

c 
if (fgets(filename, sizeof(filename), stdin) == NULL) { fprintf(stderr, "Error al leer el nombre del archivo\n"); return; }


*Apertura del Archivo en Modo Binario para Archivos de Texto*

*Descripción del Problema:*

El archivo se abre en modo binario "rb", lo que puede no ser apropiado para archivos de texto y puede afectar la interpretación de caracteres de nueva línea en diferentes sistemas operativos.

*Propuesta de Solución:*

Abrir el archivo en modo de texto "r" para asegurar que las conversiones de fin de línea se manejan correctamente:

c
FILE* f = fopen(filename, "r"); 


*Códigos de Escape ANSI Pueden No Ser Compatibles en Todas las Plataformas*

*Descripción del Problema:*

Los códigos de escape ANSI para colores pueden no ser compatibles con todas las consolas, especialmente en sistemas Windows sin configuración adicional.

*Propuesta de Solución:*

Considerar la posibilidad de detectar el sistema operativo y ajustar el uso de colores en consecuencia. Para mayor compatibilidad, se puede utilizar una biblioteca multiplataforma como ncurses, o evitar el uso de colores si no es esencial.

*Uso de Números Mágicos y Tamaños de Buffer Fijos*

*Descripción del Problema:*

Los tamaños de buffer codificados directamente como BUF_SIZE, 500 y 499 están dispersos en el código, lo que puede dificultar el mantenimiento y la comprensión.

*Propuesta de Solución:*

Definir constantes o macros para estos valores y utilizarlas consistentemente en todo el código:

c 
#define FILENAME_MAX_LENGTH 500 #define INPUT_BUFFER_SIZE 4000


Esto mejora la legibilidad y facilita cambios futuros en los tamaños de los buffers.

*Falta de Manejo Específico de Errores*

*Descripción del Problema:*

El código no maneja de manera específica los posibles errores que pueden ocurrir durante la lectura del archivo o la entrada del usuario, lo que puede dificultar la depuración y la experiencia del usuario.

*Propuesta de Solución:*

Implementar mensajes de error más detallados y manejar casos específicos, como archivos inexistentes, permisos insuficientes o entradas inválidas. Esto puede incluir el uso de perror para imprimir mensajes de error del sistema:

c 
if (!f) { perror("Error al abrir el archivo"); return; }


</details>


<img src="uax_logo_nuevo.png" alt="UAX Logo" width="400">

