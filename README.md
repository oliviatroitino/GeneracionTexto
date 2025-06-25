# GeneracionTexto
Este proyecto abarca a tres textos de la literatura española, entrena un modelo en un notebook por texto literario y los pone a conversar en un cuarto notebook.
**Esquema del proyecto**
```
|_ 3_textos_literatura_española
|   |_ La Celestina.txt
|   |_ Lazarillo-De-Tormes.txt
|   |_ quijote.txt
|_ Tarea2_Celestina.ipynb # entrenamiento y generación de texto de la Celestina
|_ Tarea2_Lazarillo.ipynb # entrenamiento y generación de texto del Lazarillo de Tormes
|_ Tarea2_Quijote.ipynb # entrenamiento y generación de texto del Quijote
|_ Interaccion.ipynb # interacción entre los tres modelos
|_ README.md
```

## Conclusiones finales por notebook
### La Celestina
Este notebook fue el primero en ser ejecutado y el que sirvió como base para determinar donde empezar con los otros modelos. Empezó con 100 épocas y 0.3 de temperatura sin cambios al notebook base. Al principio pensé que los problemas podrían llegar a ser con la cantidad de entrenamiento que recibe el modelo, e incrementé de 100 a 300 épocas. Aún así el modelo no lograba formar frases coherentes, y al poner el EarlyStopping me dí cuenta que la gramática mejoraba bastante, no por la cantidad de épocas pero por la incorporación del BatchNormalization y la reduccio2n del Dropout. El mejor intento fue el que tuvo 89 épocas (early stopping) y 0.25 de temperatura.

### Lazarillo de Tormes
Este notebook fue el segundo en ser ejecutado y se puede ver que tiene menos intentos. Esto es debido a la experiencia con el notebook de la Celestina, directamente implementé el EarlyStopping, la incorporación del BatchNormalization y la reducción del Dropout. El mejor intento fue el que tuvo 100 épocas y 0.35 de temperatura, no es completamente coherente a nivel global pero a nivel gramático y de imitación del estilo original del texto es lo mejor que pude conseguir.

### Quijote
Este notebook fue el que menos intentos tuvo, no solo por lo que tardaba en ejecutar (50 épocas tardaban lo mismo que 300 para el modelo de la Celestina, alrededor de 15 horas) pero también porque con esa cantidad de épocas lograba un modelo bastante aceptable, al nivel de los modelos finales de La Celestina y Lazarillo.

### Interacción
Este notebook fue el que más me costó, porque los modelos se degradaban muy rápidamente y empezaban a generar símbolos en vez de palabras. Al final, lo que tuve que hacer fue agregar lo siguiente a la función de generación de texto:
``` python
for layer in model.layers:
        if hasattr(layer, "reset_states"):
            layer.reset_states()
```
Lo que hace esto es recorrer todas las capas del modelo y si alguna de ellas tiene el método `reset_states()` como los que tienen capas recurrentes tipo LSTM o GRU cuando se usan con stateful=True, lo llama para reiniciar su estado interno. Esto, y agregar un default como start-string si el que se agarra del modelo anterior es muy corto, logró resolver los problemas que tenía.