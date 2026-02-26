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

import bpy
from math import radians, sin, cos
from mathutils import Vector  
def crear_material(nombre, color_rgb):
    mat = bpy.data.materials.new(name=nombre)
    mat.diffuse_color = (*color_rgb, 1.0)
    return mat

def generar_pasillo_curvo():
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()

    mat_gris = crear_material("GrisSuave", (0.5, 0.5, 0.5))
    mat_rosa = crear_material("RosaPastel", (0.95, 0.65, 0.8))

    segmentos = 20
    distancia_por_segmento = 2.0
    radio_curva = 12.0
    angulo_total = radians(90)
    angulo_por_segmento = angulo_total / (segmentos - 1)

    ancho = 4.0
    angulo_actual = 0.0
    pos_x = 0.0
    pos_y = 0.0

    trayectoria_camara = []

    for i in range(segmentos):

        dir_x = cos(angulo_actual)
        dir_y = sin(angulo_actual)

        centro_x = pos_x + dir_x * (distancia_por_segmento / 2)
        centro_y = pos_y + dir_y * (distancia_por_segmento / 2)

        trayectoria_camara.append((centro_x, centro_y, 1.6))

        offset_izq_x = -dir_y * (ancho / 2)
        offset_izq_y = dir_x * (ancho / 2)

        bpy.ops.mesh.primitive_cube_add(location=(
            centro_x + offset_izq_x,
            centro_y + offset_izq_y,
            1.0
        ))

        pared_izq = bpy.context.active_object
        pared_izq.rotation_euler.z = angulo_actual

        if i % 2 == 0:
            pared_izq.data.materials.append(mat_gris)
        else:
            pared_izq.data.materials.append(mat_rosa)
            pared_izq.scale.z = 1.5

        offset_der_x = dir_y * (ancho / 2)
        offset_der_y = -dir_x * (ancho / 2)

        bpy.ops.mesh.primitive_cube_add(location=(
            centro_x + offset_der_x,
            centro_y + offset_der_y,
            1.0
        ))

        pared_der = bpy.context.active_object
        pared_der.rotation_euler.z = angulo_actual
        pared_der.data.materials.append(mat_gris)

        pos_x += dir_x * distancia_por_segmento
        pos_y += dir_y * distancia_por_segmento

        angulo_actual += angulo_por_segmento

    bpy.ops.mesh.primitive_plane_add(size=1, location=(pos_x/2, pos_y/2, 0))
    suelo = bpy.context.active_object
    suelo.scale.x = radio_curva * 2.5
    suelo.scale.y = radio_curva * 2.5
    suelo.data.materials.append(mat_gris)

    bpy.ops.object.light_add(type='POINT', location=(pos_x, pos_y, 8))
    luz = bpy.context.active_object
    luz.data.energy = 1500

    bpy.ops.object.camera_add(location=trayectoria_camara[0])
    camara = bpy.context.active_object
    bpy.context.scene.camera = camara

    bpy.context.scene.frame_start = 1
    bpy.context.scene.frame_end = len(trayectoria_camara) * 10

    frame = 1

    for i, posicion in enumerate(trayectoria_camara):

        camara.location = posicion
        camara.keyframe_insert(data_path="location", frame=frame)

        if i < len(trayectoria_camara) - 1:

            actual = Vector(posicion)
            siguiente = Vector(trayectoria_camara[i + 1])

            direccion = siguiente - actual
            rotacion = direccion.to_track_quat('-Z', 'Y').to_euler()

            camara.rotation_euler = rotacion
            camara.keyframe_insert(data_path="rotation_euler", frame=frame)

        frame += 10

generar_pasillo_curvo()
