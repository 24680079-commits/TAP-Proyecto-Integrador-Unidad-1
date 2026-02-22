# Proyecto-Integrador-Unidad-1


# Desarrollo del Proyecto Integrador – Unidad 1
# Codigó inicial
```bash
import flet as ft

def main(page: ft.Page):
    # Configuración de página para entorno Web/Pyodide
    page.title = "Registro de Estudiantes - Tópicos Avanzados"
    page.bgcolor = "#FDFBE3"  # Fondo crema de la imagen
    page.padding = 30
    page.theme_mode = ft.ThemeMode.LIGHT

    # --- CONTROLES DE ENTRADA (Subtema 1.4) ---
    txt_nombre = ft.TextField(label="Nombre", border_color="#4D2A32",expand=True)
    txt_control = ft.TextField(label="Numero de control", border_color="#4D2A32", expand=True)
    txt_email = ft.TextField(label="Email", border_color="#4D2A32")

    dd_carrera = ft.Dropdown(
        label="Carrera",
        expand=True,
        border_color="#4D2A32",
        options=[
            ft.dropdown.Option("Ingeniería en Sistemas"),
            ft.dropdown.Option("Ingeniería Civil"),
            ft.dropdown.Option("Ingeniería Industrial"),
        ]
    )

    dd_semestre = ft.Dropdown(
        label="Semestre",
        expand=True,
        border_color="#4D2A32",
        options=[ft.dropdown.Option(str(i)) for i in range(1, 7)]
    )
    
    # Campos que se integrarán como Dropdowns posteriormente
    txt_carrera = ft.TextField(label="Carrera", expand=True, border_color="#4D2A32")
    txt_semestre = ft.TextField(label="Semestre", expand=True, border_color="#4D2A32")

    # Contenedor para Genero (Se integrará Radio posteriormente)
    # Por ahora mantenemos la estructura visual con Texto
    row_genero = ft.Row([
        ft.Text("Genero:", color="#4D2A32", weight=ft.FontWeight.BOLD),
        ft.Text("Macho / Hembra / Otro (Espacio para Radio)")
    ], alignment=ft.MainAxisAlignment.START)

    # Botón Enviar adaptado a versión 0.80.6.dev (usando content)
    btn_enviar = ft.ElevatedButton(
        content=ft.Text("Enviar", color="black", size=16),
        bgcolor=ft.Colors.GREY_500,
        width=page.width, # Ocupa el ancho disponible
        style=ft.ButtonStyle(
            shape=ft.RoundedRectangleBorder(radius=0),
        )
    )

    # --- CONSTRUCCIÓN DE LA INTERFAZ (Subtema 1.1) ---
    page.add(
        ft.Column([
            txt_nombre,
            txt_control,
            txt_email,
            # Fila para Carrera y Semestre
            ft.Row([
                dd_carrera,
                dd_semestre
            ], spacing=10),
            # Espacio para el Genero
            row_genero,
            # Botón final
            btn_enviar
        ], spacing=15)
    )

Ejecución específica para visualización en Navegador
ft.app(target=main, view=ft.AppView.WEB_BROWSER)
```
# Etapa Inicial: Código Base Proporcionado

El código base proporcionado por el profesor tenía como objetivo construir únicamente la estructura visual de la interfaz gráfica, correspondiente al subtema:

Creación de interfaz gráfica (1.1)

Componentes gráficos de control (1.4)

El sistema incluía:

TextField (Nombre, Número de control, Email)

Dropdown (Carrera y Semestre)

Texto simulado para Género (no interactivo)

ElevatedButton sin evento asociado

⚠ Importante:
El código base no incluía programación dirigida por eventos, validaciones, ni ventana modal.
La interfaz era completamente estática.

# Segunda Etapa: Implementación de Validaciones en Tiempo Real

Se agregaron funciones para validar los campos conforme el usuario escribe, utilizando el evento:

on_change=funcion_validacion
🔹 Validación del Nombre
def validar_nombre(e):

Se verifica que:

No esté vacío.

Solo contenga letras y espacios.

Se utiliza:

valor.replace(" ", "").isalpha()

Si el dato es incorrecto, se muestra un mensaje dinámico mediante:

txt_nombre.error_text = "Solo se permiten letras"

Esto mejora la experiencia del usuario al mostrar errores inmediatamente.

🔹 Validación del Número de Control

Se implementó:

valor.isdigit()

Permitiendo únicamente valores numéricos.

🔹 Validación del Email

Se incorporó el módulo:

import re

Y se utilizó una expresión regular:

patron = r"^[\w\.-]+@[\w\.-]+\.\w+$"

Esto asegura que el correo tenga un formato válido.

# Tercera Etapa: Reemplazo del Texto por Control Radio

En el código base, el género era solo texto estático:

"Macho / Hembra / Otro (Espacio para Radio)"

Se reemplazó por un control interactivo real:

radio_genero = ft.RadioGroup(...)

Ahora el usuario puede seleccionar una opción única.

Esto cumple con el requisito:

✔ Inclusión de control Radio
✔ Manejo de componentes gráficos de control

# Cuarta Etapa: Validación General Antes del Envío

Se creó una función central:

def validar_general():

Esta función verifica que:

No haya campos vacíos

El nombre sea válido

El número de control sea numérico

El email tenga formato correcto

Se haya seleccionado carrera

Se haya seleccionado semestre

Se haya seleccionado género

Esta función devuelve un valor booleano.

En el botón:

if not validar_general():
    return

Si algún dato es incorrecto, el formulario no se envía.

# Quinta Etapa: Implementación del Evento del Botón

En el código base el botón no tenía evento.

Se agregó:

on_click=enviar

Esto implementa la programación dirigida por eventos, donde:

El sistema permanece en espera.

Al hacer clic, se ejecuta la función enviar().

# Sexta Etapa: Implementación de Ventana Modal (AlertDialog)

Se creó dinámicamente un objeto:

ft.AlertDialog()

El contenido del diálogo muestra los datos capturados:

ft.Text(f"Nombre: {txt_nombre.value}")

Se añadió el diálogo al overlay de la página:

page.overlay.append(dialogo)

Y se activa con:

dialogo.open = True
page.update()

También se implementó una función para cerrarlo:

def cerrar_dialogo(dialogo):

Esto cumple el requisito:

✔ Mostrar datos en ventana modal después de enviar.

# Codigó final
```bash
import flet as ft
import re

def main(page: ft.Page):

    page.title = "Registro de Estudiantes - Tópicos Avanzados"
    page.bgcolor = "#FDFBE3"
    page.padding = 30
    page.theme_mode = ft.ThemeMode.LIGHT

    # ---------- FUNCIONES VALIDACIÓN EN TIEMPO REAL ----------

    def validar_nombre(e):
        valor = txt_nombre.value

        if not valor:
            txt_nombre.error_text = None
        elif not valor.replace(" ", "").isalpha():
            txt_nombre.error_text = "Solo se permiten letras"
        else:
            txt_nombre.error_text = None

        txt_nombre.update()

    def validar_control(e):
        valor = txt_control.value

        if not valor:
            txt_control.error_text = None
        elif not valor.isdigit():
            txt_control.error_text = "Solo se permiten números"
        else:
            txt_control.error_text = None

        txt_control.update()

    def validar_email(e):
        valor = txt_email.value
        patron = r"^[\w\.-]+@[\w\.-]+\.\w+$"

        if not valor:
            txt_email.error_text = None
        elif not re.match(patron, valor):
            txt_email.error_text = "Email no válido"
        else:
            txt_email.error_text = None

        txt_email.update()

    # ---------- CAMPOS ----------

    txt_nombre = ft.TextField(
        label="Nombre",
        border_color="#4D2A32",
        expand=True,
        on_change=validar_nombre
    )

    txt_control = ft.TextField(
        label="Numero de control",
        border_color="#4D2A32",
        expand=True,
        on_change=validar_control
    )

    txt_email = ft.TextField(
        label="Email",
        border_color="#4D2A32",
        on_change=validar_email
    )

    dd_carrera = ft.Dropdown(
        label="Carrera",
        expand=True,
        border_color="#4D2A32",
        options=[
            ft.dropdown.Option("Ingeniería en Sistemas"),
            ft.dropdown.Option("Ingeniería Civil"),
            ft.dropdown.Option("Ingeniería Industrial"),
        ]
    )

    dd_semestre = ft.Dropdown(
        label="Semestre",
        expand=True,
        border_color="#4D2A32",
        options=[ft.dropdown.Option(str(i)) for i in range(1, 7)]
    )

    radio_genero = ft.RadioGroup(
        content=ft.Row(
            [
                ft.Radio(value="Macho", label="Macho"),
                ft.Radio(value="Hembra", label="Hembra"),
                ft.Radio(value="Otro", label="Otro"),
            ],
            spacing=20
        )
    )

    row_genero = ft.Column([
        ft.Text("Género:", weight=ft.FontWeight.BOLD),
        radio_genero
    ])

    # ---------- VALIDACIÓN GENERAL ----------

    def validar_general():
        patron = r"^[\w\.-]+@[\w\.-]+\.\w+$"

        return (
            txt_nombre.value
            and txt_nombre.value.replace(" ", "").isalpha()
            and txt_control.value
            and txt_control.value.isdigit()
            and txt_email.value
            and re.match(patron, txt_email.value)
            and dd_carrera.value
            and dd_semestre.value
            and radio_genero.value
        )

    def cerrar_dialogo(dialogo):
        dialogo.open = False
        page.update()

    def enviar(e):

        if not validar_general():
            return

        dialogo = ft.AlertDialog(
            modal=True,
            title=ft.Text("Información Registrada"),
            content=ft.Column(
                [
                    ft.Text(f"Nombre: {txt_nombre.value}"),
                    ft.Text(f"No. Control: {txt_control.value}"),
                    ft.Text(f"Email: {txt_email.value}"),
                    ft.Text(f"Carrera: {dd_carrera.value}"),
                    ft.Text(f"Semestre: {dd_semestre.value}"),
                    ft.Text(f"Género: {radio_genero.value}")
                ],
                tight=True,
            ),
            actions=[
                ft.TextButton(
                    "Cerrar",
                    on_click=lambda e: cerrar_dialogo(dialogo)
                )
            ],
        )

        page.overlay.append(dialogo)
        dialogo.open = True
        page.update()

    btn_enviar = ft.ElevatedButton(
        content=ft.Text("Enviar"),
        on_click=enviar,
        width=page.width
    )

    page.add(
        ft.Column(
            [
                txt_nombre,
                txt_control,
                txt_email,
                ft.Row([dd_carrera, dd_semestre], spacing=10),
                row_genero,
                btn_enviar
            ],
            spacing=15
        )
    )

ft.app(target=main, view=ft.AppView.WEB_BROWSER)


```
<img width="1062" height="983" alt="Captura de pantalla 2026-02-22 a la(s) 11 45 05" src="https://github.com/user-attachments/assets/1fd43556-ce75-4674-b50d-662d8874b2b4" />

