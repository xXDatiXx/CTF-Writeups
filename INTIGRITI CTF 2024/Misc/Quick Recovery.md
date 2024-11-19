# Quick Recovery
## Challenge
- Misc
- 1337UP LIVE 2024
### Description
Hey, check this QR code ASAP! It's highly sensitive so I scrambled it, but you shouldn't have a hard time reconstructing - just make sure to update the a_order to our shared PIN. The b_order is the reverse of that 😉

We get an obscured QR and the code and the code they used to hide the QR:

```Python
from PIL import Image, ImageDraw
from itertools import permutations
import subprocess

qr_code_image = Image.open("qr_code.png")
width, height = qr_code_image.size
half_width, half_height = width // 2, height // 2

squares = {
    "1": (0, 0, half_width, half_height),
    "2": (half_width, 0, width, half_height),
    "3": (0, half_height, half_width, height),
    "4": (half_width, half_height, width, height)
}


def split_square_into_triangles(img, box):
    x0, y0, x1, y1 = box
    a_triangle_points = [(x0, y0), (x1, y0), (x0, y1)]
    b_triangle_points = [(x1, y1), (x1, y0), (x0, y1)]

    def crop_triangle(points):
        mask = Image.new("L", img.size, 0)
        draw = ImageDraw.Draw(mask)
        draw.polygon(points, fill=255)
        triangle_img = Image.new("RGBA", img.size)
        triangle_img.paste(img, (0, 0), mask)
        return triangle_img.crop((x0, y0, x1, y1))

    return crop_triangle(a_triangle_points), crop_triangle(b_triangle_points)


triangle_images = {}
for key, box in squares.items():
    triangle_images[f"{key}a"], triangle_images[f"{key}b"] = split_square_into_triangles(
        qr_code_image, box)

a_order = ["1", "2", "3", "4"]  # UPDATE ME
b_order = ["1", "2", "3", "4"]  # UPDATE ME

final_positions = [
    (0, 0),
    (half_width, 0),
    (0, half_height),
    (half_width, half_height)
]

reconstructed_image = Image.new("RGBA", qr_code_image.size)

for i in range(4):
    a_triangle = triangle_images[f"{a_order[i]}a"]
    b_triangle = triangle_images[f"{b_order[i]}b"]
    combined_square = Image.new("RGBA", (half_width, half_height))
    combined_square.paste(a_triangle, (0, 0))
    combined_square.paste(b_triangle, (0, 0), b_triangle)
    reconstructed_image.paste(combined_square, final_positions[i])

reconstructed_image.save("obscured.png")
print("Reconstructed QR code saved as 'obscured.png'")
```
![obscured](https://github.com/user-attachments/assets/90e19b18-2ca5-4b8a-a0a4-9af7fa1a2592)

### Objective
Obtain the original QR.

## Analysis
The provided code takes a QR code image and splits it into four quadrants, then further splits each quadrant into two triangles labeled 'a' and 'b'. The challenge lies in the reordering and recombination of these triangles based on the provided instructions to reconstruct the original QR code.

The QR image is divided into four quadrants:
- Quadrant 1: Top-left
- Quadrant 2: Top-right
- Quadrant 3: Bottom-left
- Quadrant 4: Bottom-right

Each quadrant is then divided into two triangles:

- Triangle a: Top-left triangle of the quadrant.
- Triangle b: Bottom-right triangle of the quadrant.

The a_order and b_order lists define how the triangles are rearranged in the final reconstruction.

- a_order: The order in which the 'a' triangles from each quadrant are used.
- b_order: The order in which the 'b' triangles are used, which must be the reverse of a_order as indicated in the challenge description.

## Solution
Make a code that gives all te combinations
```Python
from PIL import Image, ImageDraw
from itertools import permutations
import os

# Cargar la imagen del código QR
qr_code_image = Image.open("/home/dati/Desktop/UP/CTF-Labs/qrecovery/obscured.png")
width, height = qr_code_image.size
half_width, half_height = width // 2, height // 2

# Definir los cuadrantes
squares = {
    "1": (0, 0, half_width, half_height),
    "2": (half_width, 0, width, half_height),
    "3": (0, half_height, half_width, height),
    "4": (half_width, half_height, width, height)
}

# Crear el directorio de salida si no existe
output_dir = "/home/dati/Desktop/UP/CTF-Labs/qrecovery/a"
os.makedirs(output_dir, exist_ok=True)

# Función para dividir cada cuadrante en dos triángulos
def split_square_into_triangles(img, box):
    x0, y0, x1, y1 = box
    a_triangle_points = [(x0, y0), (x1, y0), (x0, y1)]
    b_triangle_points = [(x1, y1), (x1, y0), (x0, y1)]

    def crop_triangle(points):
        mask = Image.new("L", img.size, 0)
        draw = ImageDraw.Draw(mask)
        draw.polygon(points, fill=255)
        triangle_img = Image.new("RGBA", img.size)
        triangle_img.paste(img, (0, 0), mask)
        return triangle_img.crop((x0, y0, x1, y1))

    return crop_triangle(a_triangle_points), crop_triangle(b_triangle_points)

# Dividir cada cuadrante en dos triángulos
triangle_images = {}
for key, box in squares.items():
    triangle_images[f"{key}a"], triangle_images[f"{key}b"] = split_square_into_triangles(
        qr_code_image, box)

# Todas las permutaciones posibles para los órdenes de los triángulos
all_orders = list(permutations(["1", "2", "3", "4"]))

# Posiciones finales para pegar los cuadrantes reconstruidos
final_positions = [
    (0, 0),
    (half_width, 0),
    (0, half_height),
    (half_width, half_height)
]

# Probar todas las combinaciones posibles de a_order y b_order
for a_order in all_orders:
    for b_order in all_orders:
        # Crear una nueva imagen para cada intento
        reconstructed_image = Image.new("RGBA", qr_code_image.size)

        for i in range(4):
            a_triangle = triangle_images[f"{a_order[i]}a"]
            b_triangle = triangle_images[f"{b_order[i]}b"]
            combined_square = Image.new("RGBA", (half_width, half_height))
            combined_square.paste(a_triangle, (0, 0))
            combined_square.paste(b_triangle, (0, 0), b_triangle)
            reconstructed_image.paste(combined_square, final_positions[i])

        # Guardar cada intento de reconstrucción en el directorio especificado
        filename = os.path.join(output_dir, f"reconstructed_{''.join(a_order)}_{''.join(b_order)}.png")
        reconstructed_image.save(filename)
        print(f"Saved: {filename}")

#2413_3142
```
Searching in my files I founded:

![image](https://github.com/user-attachments/assets/8d97fbc8-2c6c-4343-b15d-f3cac7280196)

When ypu read the correct CQ you get:

![image](https://github.com/user-attachments/assets/50395a79-b523-4393-95e9-e1938d186e82)

flag: `INTIGRITI{7h475_h0w_y0u_r3c0n57ruc7_qr_c0d3}`

