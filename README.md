# Proyecto-Integrador
En el siguiente repositorio se muestra un código con animación   
<p>En el siguiente repositorio podras encontar un código en Python en Blender con el quer podras construir un 
escenario Procedural para posteriormente curvearlo y animar una camara que permita ver el recorrido de este mismo "pasillo"</p>
<p>Objetivo: Implementar transformaciones tridimensionales (traslación y escalamiento) y
modelos de color mediante scripting en Blender.</p>

# Paso 1. Limpieza del Entorno y Preparación 
<p>Antes de construir, debemos asegurar que la escena esté vacía. Esto enseña a los alumnos la
importancia de la gestión de objetos en memoria.</p>
<p>Lógica: Seleccionar todo y borrar.</p>
<img width="249" height="74" alt="image" src="https://github.com/user-attachments/assets/2ff581db-6989-4b4c-85e5-a3209515ef2b"/>

# Paso 2: Definición de Materiales (Modelos de Color)
<p>Según el tema 1.4 (Modelos del color) , utilizaremos el modelo RGB para dar vida al
escenario.</p>
<p> Lógica: Crear una función que genere materiales para no repetir código (fomentando el
paradigma orientado a objetos).</p>
<img width="341" height="130" alt="image" src="https://github.com/user-attachments/assets/f325b088-b610-4b6a-9b34-858644ac649f" />

# Paso 3: Construcción de Paredes con Ciclos (Transformaciones)
<p>Aquí aplicamos el Tema 3.3.1 (Traslación). Usaremos un ciclo for para automatizar la
creación de un pasillo.</p>
<p> Lógica: Iterar para colocar cubos en el eje Y, alternando materiales según el índice del
ciclo.</p>
<img width="435" height="227" alt="image" src="https://github.com/user-attachments/assets/dd1dbeba-5877-4590-84ba-d5aa5b04507a" />

# Paso 4: Completando la Geometría (Simetría y Suelo)
<p>Para que parezca un videojuego, cerramos el pasillo y agregamos una superficie base.</p>
<p> Lógica: Replicar la pared en el lado positivo del eje X y añadir un plano escalado para el
suelo.</p>
<img width="405" height="175" alt="image" src="https://github.com/user-attachments/assets/dc84a5c5-f604-4076-bc58-0651e1bdc7ca" />

# Paso 5: Iluminación Básica (Tema 4.2)
<p>Un escenario de videojuego no está completo sin luz.</p>
<p> Lógica: Agregar una luz de tipo "Point" para iluminar el recorrido.</p>
<img width="387" height="75" alt="image" src="https://github.com/user-attachments/assets/8d4dae30-54bd-4af0-b6b5-49026b223f87" />
<p>En el siguinete codigo se puede observar el cambio de color de los cubos así com tamvbien la curbatura y la animación</p>

