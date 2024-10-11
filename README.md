# Unidad-3

## Actividad # 1: introducción a los protocolos binarios - caso de estudio
Durante la lectura, intenta dar respuestas a las siguientes preguntas:

- ¿Cómo se ve un protocolo binario?
  Un protocolo binario se refiere a una forma de comunicación en la que los datos se transmiten como secuencias de bits (ceros y unos) en lugar de texto legible por humanos.
  Los mensajes que se envían utilizando un protocolo binario son compactos y no necesariamente comprensibles a simple vista, ya que están codificados en forma binaria.
- ¿Puedes describir las partes de un mensaje?¿Para qué sirve cada parte del mensaje?
  Sus partes serian el Head, los datos, Y la cola
  - El head sirve para dar instrucciones al receptor sobre cómo debe interpretar los datos que siguen.
  - Los Datos contienen la información principal que el emisor desea enviar.
  - La cola ayuda a indicar el final del mensaje o verificar su integridad.

## Actividad # 2: experimento
en este punto, hagamos un repaso de las funciones que han apoyado la comunicación seral:
- Serial.available()
  Esta función devuelve el número de bytes disponibles para ser leídos desde el buffer serial. Es útil para verificar si hay datos entrantes antes de intentar leerlos.
- Serial.read()
  Lee el siguiente byte de datos disponible en el puerto serial. Si no hay datos disponibles, devuelve -1. Es útil para leer byte a byte desde el buffer serial.
- Serial.readBytes(buffer, length)
  Lee una cantidad de bytes especificada desde el puerto serial y los almacena en un buffer. La lectura termina después de recibir la cantidad de bytes indicada o cuando se agota el tiempo de espera.
- Serial.write()
  Envía datos al puerto serial. Puede enviar un solo byte, una cadena de texto, o una serie de bytes. Es utilizado para transmitir datos desde Arduino hacia otro dispositivo.
Nótese que la siguiente función no está en la lista de repaso:

- `Serial.readBytesUntil()` ¿Sospecha por qué se ha excluido? La razón es porque en un protocolo binario usualmente no tiene un carácter de FIN DE MENSAJE, como si ocurre con los protocolos ASCII, 
donde usualmente el último carácter es el `\n`.

La razón por la cual la función Serial.readBytesUntil() ha sido excluida en el repaso es porque esta función está diseñada para leer datos del puerto serial hasta que se 
encuentre un carácter de terminación específico, lo cual no es útil en protocolos binarios. En protocolos binarios, no hay un carácter definido que marque el fin de un 
mensaje, como ocurre en los protocolos ASCII, donde comúnmente se utiliza el carácter \n (nueva línea) como delimitador de fin de mensaje.





void setup() {
    Serial.begin(115200);
}

void loop() {
    // Número flotante a transmitir
    static float num = 3589.3645;
    static float num2 = 4889.3445;

    // Verifica si hay datos disponibles para leer
    if (Serial.available()) {
        char command = Serial.read();

        // Si se recibe 's', transmite el número en little-endian
        if (command == 's') {
            Serial.write((uint8_t*)&num, 4); // Little-endian, por defecto en sistemas como Arduino
        }

        // Si se recibe 'b', transmite el número en big-endian
        if (command == 'b') {
            enviarBigEndian(num2); // Llamada a la función que envía en big-endian
        }
    }
}

// Función para enviar un número en formato big-endian
void enviarBigEndian(float numero) {
    uint8_t *bytePointer = (uint8_t *) &numero;  // Apunta al número como un arreglo de bytes

    // Enviar los bytes en orden inverso (big-endian)
    for (int i = 3; i >= 0; i--) {
        Serial.write(bytePointer[i]);
    }
}
