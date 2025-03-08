# combi

import itertools

def encontrar_mejor_combinacion(precios, rendimientos, presupuesto_max):
    productos = list(zip(precios, rendimientos))
    n_productos = len(productos)
    
    # Generamos todas las combinaciones posibles de cantidades (0 a 19) para cada producto
    cantidades_posibles = [range(20)] * n_productos
    todas_combinaciones = itertools.product(*cantidades_posibles)
    
    mejor_combinacion = None
    mejor_rendimiento_por_dolar = 0
    
    print("Evaluando combinaciones:")
    for cantidades in todas_combinaciones:
        precio_total = sum(productos[i][0] * cantidades[i] for i in range(n_productos))
        # Calculamos el rendimiento real en dólares (precio * (rendimiento/100)) por cantidad
        rendimiento_total = sum(
            cantidades[i] * productos[i][0] * (productos[i][1] / 100)
            for i in range(n_productos)
        )
        
        # Filtramos combinaciones válidas (precio > 0 para evitar división por cero)
        if pres_minimo < precio_total <= presupuesto_max:
            rendimiento_dolar = rendimiento_total / precio_total
            if rendimiento_dolar > mejor_rendimiento_por_dolar:
                mejor_rendimiento_por_dolar = rendimiento_dolar
                mejor_combinacion = cantidades
                print(f"Combinación: {cantidades}, Rendimiento: ${rendimiento_total:.2f}")
    
    # Mostrar resultados
    if mejor_combinacion:
        productos_str = []
        for i, cantidad in enumerate(mejor_combinacion):
            if cantidad > 0:
                productos_str.append(
                    f"Producto {i} x {cantidad}u. (${precios[i]} c/u, {rendimientos[i]}%)"
                )
        
        precio_total = sum(precios[i] * mejor_combinacion[i] for i in range(n_productos))
        rendimiento_total = sum(
            mejor_combinacion[i] * precios[i] * (rendimientos[i] / 100)
            for i in range(n_productos)
        )

        print(f"\nMejor combinación: {', '.join(productos_str)}")
        print(f"Inversión total: ${precio_total}")
        print(f"Rendimiento total: ${rendimiento_total:.2f}")
        print(f"Rendimiento por dólar invertido: ${mejor_rendimiento_por_dolar:.2f}")
    else:
        print("No se encontró ninguna combinación válida dentro del presupuesto")

# Ejemplo de uso
precios = [40, 400, 60]
rendimientos = [90, 60, 75]
presupuesto_max = 5400
pres_minimo=5000

encontrar_mejor_combinacion(precios, rendimientos, presupuesto_max)
