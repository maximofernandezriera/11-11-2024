# 11-11-2024

```java

        /*
         *  Empleo únicamente bucles y arrays.
         */

    // ------ Ejercicio 1 ------
    // Verifica que un NIF tiene formato correcto:
    // - 8 primeros caracteres deben ser dígitos
    // - El último una letra mayúscula distinta de 'Ñ'
    static boolean tieneFormatoNIF(char[] a) {
        /*
         *  La longitud debe ser exactamente 9: 8 dígitos + 1 letra
         *  Si no es así, directamente devolvemos false.
         */
        if (a == null || a.length != 9) return false;
        /*
         * Comprobamos individualmente cada uno de los 8 primeros dígitos
         * Si alguno no cumple, salimos antes con false.
         */
        for (int i = 0; i < 8; i++) {
            if (!esDigito(a[i])) return false;
        }
        /*
         *  El último carácter debe ser una letra entre 'A'-'Z', pero además
         *  explícitamente se excluye 'Ñ'.
         */
        char ultima = a[8];
        return esLetraMayuscula(ultima) && ultima != 'Ñ';
        /*
         *  Este ejercicio utiliza solo comprobaciones directas y bucles,
         *  respetando el uso de arrays de longitud fija.
         */
    }

    static boolean esLetraMayuscula(char c) {
        return 'A' <= c && c <= 'Z';
    }
    static boolean esDigito(char c) {
        return '0' <= c && c <= '9';
    }

    // ------ Ejercicio 2 ------
    // Devuelve el número entero representado por los 8 primeros dígitos del NIF
    static int aDigito(char c) {
        return c - '0';
    }
    static long aNum(char[] a) {
        /*
         *  Usamos un acumulador de tipo long por seguridad, ya que 8 dígitos 
         *  pueden superar el límite de int. Extraemos cada dígito y construimos
         *  el número como haríamos manualmente (después del chequeo de formato).
         */
        long valor = 0L;
        for (int i = 0; i < 8; i++) {
            valor = valor * 10 + aDigito(a[i]); // Acumulación decimal
        }
        return valor;
        /*
         * Este ejercicio evita métodos externos como Integer.parseInt y respeta
         * el uso de arrays fijos y operaciones simples.
         */
    }

    // ------ Ejercicio 3 ------
    // Comprueba si un NIF no solo tiene formato, sino que la letra es la correcta
    static final char[] TABLA_NIF = new char[]{
        'T','R','W','A','G','M','Y','F','P','D','X',
        'B','N','J','Z','S','Q','V','H','L','C','K','E'
    };
    static char letraDeResto(int resto) {
        return TABLA_NIF[resto];
    }
    static boolean esNIFCorrecto(char[] a) {
        /*
         *  Primero se valida el formato con tieneFormatoNIF()
         *  Luego se calcula el número, el resto de la división entre 23,
         *  y se compara la letra esperada con la real.
         */
        if (!tieneFormatoNIF(a)) return false; // Chequea formato
        long num = aNum(a);                   // Obtiene los dígitos como número
        int resto = (int)(num % 23);          // Calcula el resto módulo 23
        char letraEsperada = letraDeResto(resto);
        return a[8] == letraEsperada;
        /*
         *  Así aseguramos que el NIF es válido tanto en forma como en letra de control.
         */
    }

    // ------ Ejercicio 4 ------
    // Verifica si el array de enteros es una "colección pseudodómino"
    static boolean esPseudoDomino(int[] a) {
        /*
         *  Un pseudodómino consiste en pares encadenados:
         *  - Si la longitud es cero o dos, es trivialmente válido
         *  - La longitud debe ser par
         *  - Por cada par, el último número del actual debe ser igual al primero del siguiente.
         */
        if (a == null) return false;
        int n = a.length;
        if (n == 0 || n == 2) return true;
        if (n % 2 != 0) return false; // Solo arrays de longitud par
        for (int i = 0; i + 3 < n; i += 2) {
            int finParActual = a[i + 1];
            int inicioParSiguiente = a[i + 2];
            if (finParActual != inicioParSiguiente) return false;
        }
        return true;
        /*
         *  El recorrido y la comparación se hace usando índices, siguiendo las restricciones
         *  impuestas del uso de arrays de longitud fija.
         */
    }

    // ------ Ejercicio 5 ------
    // Crea la imagen espejo de un array, es decir: [original ... invertido]
    static int[] imagenEspejo(int[] a) {
        /*
         *  Reserva un array de doble longitud. Copia primero de izquierda a derecha,
         *  después toma los elementos del original en orden inverso y los añade.
         *  Así se obtiene la "imagen espejo". Si el array está vacío, se lanza excepción.
         */
        if (a == null || a.length == 0) {
            throw new IllegalArgumentException("El array debe tener al menos un elemento");
        }
        int n = a.length;
        int[] res = new int[2 * n];
        // Copiar parte original
        for (int i = 0; i < n; i++) {
            res[i] = a[i];
        }
        // Copiar parte invertida
        for (int i = 0; i < n; i++) {
            res[n + i] = a[n - 1 - i];
        }
        return res;

    }


