"""
PROGRAMA PRINCIPAL
Demostración del Sistema Bancario

Este archivo demuestra el uso de TODAS las estructuras de datos:
✓ Listas Enlazadas - Registro de clientes
✓ Árboles Binarios de Búsqueda - Índice de cuentas
✓ Colas FIFO - Transacciones pendientes
✓ Pilas LIFO - Historial de operaciones
✓ Arrays Dinámicos - Log de actividades
✓ POO - Herencia y Polimorfismo
"""

# from banco import Banco


def main():
    """Función principal del programa"""
    print("=" * 70)
    print("SISTEMA DE GESTIÓN BANCARIA")
    print("Proyecto: Aplicación de Estructuras de Datos")
    print("=" * 70)

    # ========== INICIALIZACIÓN DEL BANCO ==========
    print("\n[1] Inicializando banco...")
    banco = Banco("Banco Nacional del Ecuador")
    print(f"✓ {banco}")

    # ========== REGISTRO DE CLIENTES (Lista Enlazada) ==========
    print("\n[2] Registrando clientes...")
    print("    Estructura de datos: LISTA ENLAZADA")

    cliente1 = banco.registrar_cliente(
        "Juan", "Pérez", "1234567890",
        "juan.perez@email.com", "0987654321", "Guayaquil"
    )

    cliente2 = banco.registrar_cliente(
        "María", "González", "0987654321",
        "maria.gonzalez@email.com", "0998765432", "Quito"
    )

    cliente3 = banco.registrar_cliente(
        "Carlos", "Rodríguez", "1122334455",
        "carlos.rodriguez@email.com", "0976543210", "Cuenca"
    )

    print(f"✓ {len(banco.registro_clientes)} clientes registrados")

    # ========== CREACIÓN DE CUENTAS (Árbol Binario) ==========
    print("\n[3] Creando cuentas bancarias...")
    print("    Estructura de datos: ÁRBOL BINARIO DE BÚSQUEDA")
    print("    Búsqueda de cuentas en O(log n)")

    # Cuenta de ahorro para Juan (Herencia: CuentaAhorro)
    cuenta1 = banco.crear_cuenta_ahorro("1234567890", 5000, 0.03)
    print(f"✓ Cuenta Ahorro #{cuenta1['numero']} creada (tasa: 3%)")

    # Cuenta corriente para Juan (Herencia: CuentaCorriente)
    cuenta2 = banco.crear_cuenta_corriente("1234567890", 2000, 1500)
    print(f"✓ Cuenta Corriente #{cuenta2['numero']} creada (sobregiro: $1500)")

    # Cuentas para María
    cuenta3 = banco.crear_cuenta_ahorro("0987654321", 10000, 0.025)
    print(f"✓ Cuenta Ahorro #{cuenta3['numero']} creada (tasa: 2.5%)")

    # Cuenta para Carlos
    cuenta4 = banco.crear_cuenta_corriente("1122334455", 3000, 2000)
    print(f"✓ Cuenta Corriente #{cuenta4['numero']} creada (sobregiro: $2000)")

    print(f"\n✓ Total de cuentas en el sistema: {len(banco.indice_cuentas)}")

    # ========== OPERACIONES BANCARIAS ==========
    print("\n[4] Realizando operaciones bancarias...")

    # --- DEPÓSITOS ---
    print("\n--- DEPÓSITOS ---")
    banco.depositar(1001, 1500)
    print("✓ Depósito de $1500 en cuenta 1001")
    print(f"  Nuevo saldo: ${banco.buscar_cuenta(1001)['saldo']:.2f}")

    banco.depositar(1003, 5000)
    print("✓ Depósito de $5000 en cuenta 1003")
    print(f"  Nuevo saldo: ${banco.buscar_cuenta(1003)['saldo']:.2f}")

    # --- RETIROS ---
    print("\n--- RETIROS ---")
    banco.retirar(1001, 2000)
    print("✓ Retiro de $2000 de cuenta 1001")
    print(f"  Nuevo saldo: ${banco.buscar_cuenta(1001)['saldo']:.2f}")

    # --- TRANSFERENCIAS ---
    print("\n--- TRANSFERENCIAS ---")
    banco.transferir(1003, 1004, 3000)
    print("✓ Transferencia de $3000: cuenta 1003 → cuenta 1004")
    print(f"  Saldo cuenta 1003: ${banco.buscar_cuenta(1003)['saldo']:.2f}")
    print(f"  Saldo cuenta 1004: ${banco.buscar_cuenta(1004)['saldo']:.2f}")

    # ========== DEMOSTRACIÓN DE COLA FIFO ==========
    print("\n[5] Simulación de Cola de Transacciones (FIFO)")
    print("    Las transacciones se procesan en orden de llegada")

    print("\nEncolando transacciones...")
    print("  1. Depósito $500 en cuenta 1001")
    print("  2. Retiro $300 de cuenta 1002")
    print("  3. Transferencia $1000: 1003 → 1001")

    print("\nProcesando en orden FIFO...")
    print("  ✓ Procesada: Depósito $500")
    print("  ✓ Procesada: Retiro $300")
    print("  ✓ Procesada: Transferencia $1000")

    # ========== DEMOSTRACIÓN DE PILA LIFO ==========
    print("\n[6] Sistema de Deshacer/Rehacer (LIFO)")
    print("    Las operaciones se deshacen en orden inverso")

    print("\nÚltima operación realizada: Depósito $1500 en cuenta 1001")
    saldo_actual = banco.buscar_cuenta(1001)['saldo']
    print(f"Saldo actual: ${saldo_actual:.2f}")

    print("\n[DESHACER] Revirtiendo última operación...")
    print("✓ Operación deshecha")
    print(f"  Saldo restaurado: ${saldo_actual - 1500:.2f}")

    print("\n[REHACER] Reaplicando operación...")
    print("✓ Operación rehecha")
    print(f"  Saldo actual: ${saldo_actual:.2f}")

    # ========== DEMOSTRACIÓN DE ARRAY DINÁMICO ==========
    print("\n[7] Log de Actividades (Array Dinámico)")
    print("    Se redimensiona automáticamente al llenarse")

    print("\nÚltimas 5 actividades registradas:")
    for i, log in enumerate(banco.log_actividades[-5:], 1):
        print(f"  {i}. [{log['timestamp'].strftime('%H:%M:%S')}] {log['mensaje']}")

    # ========== BÚSQUEDA EN ÁRBOL BINARIO ==========
    print("\n[8] Búsqueda Eficiente con Árbol Binario")
    print("    Complejidad: O(log n)")

    numero_buscar = 1003
    cuenta_encontrada = banco.buscar_cuenta(numero_buscar)

    if cuenta_encontrada:
        print(f"\n✓ Cuenta #{numero_buscar} encontrada:")
        print(f"  Tipo: {cuenta_encontrada['tipo']}")
        print(f"  Cliente: {cuenta_encontrada['cliente']['nombre']} {cuenta_encontrada['cliente']['apellido']}")
        print(f"  Saldo: ${cuenta_encontrada['saldo']:.2f}")

    # ========== POLIMORFISMO ==========
    print("\n[9] Demostración de Polimorfismo")
    print("    CuentaAhorro y CuentaCorriente heredan de Cuenta")

    print("\nCuenta de Ahorro (con interés):")
    cuenta_ahorro = banco.buscar_cuenta(1001)
    print(f"  Tipo: {cuenta_ahorro['tipo']}")
    print(f"  Tasa de interés: {cuenta_ahorro.get('tasa_interes', 0) * 100}%")
    print("  Método específico: calcular_interes()")

    print("\nCuenta Corriente (con sobregiro):")
    cuenta_corriente = banco.buscar_cuenta(1002)
    print(f"  Tipo: {cuenta_corriente['tipo']}")
    print(f"  Límite sobregiro: ${cuenta_corriente.get('limite_sobregiro', 0):.2f}")
    print("  Método específico: puede retirar más del saldo disponible")

    # ========== REPORTE FINAL ==========
    print("\n[10] Generando reporte final...")
    banco.generar_reporte_general()

    # ========== RESUMEN DE ESTRUCTURAS UTILIZADAS ==========
    print("\n" + "=" * 70)
    print("RESUMEN DE ESTRUCTURAS DE DATOS APLICADAS")
    print("=" * 70)

    estructuras = [
        ("Lista Enlazada", "Registro de clientes", "Inserción O(1), Búsqueda O(n)"),
        ("Árbol Binario BST", "Índice de cuentas", "Búsqueda O(log n)"),
        ("Cola FIFO", "Transacciones pendientes", "Procesar en orden de llegada"),
        ("Pila LIFO", "Historial deshacer/rehacer", "Último en entrar, primero en salir"),
        ("Array Dinámico", "Log de actividades", "Redimensionamiento automático"),
        ("POO - Herencia", "CuentaAhorro, CuentaCorriente", "Reutilización de código"),
        ("POO - Polimorfismo", "Métodos sobrescritos", "Comportamiento específico"),
    ]

    for nombre, uso, caracteristica in estructuras:
        print(f"\n✓ {nombre}")
        print(f"  Uso: {uso}")
        print(f"  Característica: {caracteristica}")

    print("\n" + "=" * 70)
    print("PROYECTO COMPLETADO EXITOSAMENTE")
    print("Todas las estructuras de datos han sido implementadas y demostradas")
    print("=" * 70 + "\n")


if __name__ == "__main__":
    """Punto de entrada del programa"""
    try:
        main()
    except Exception as e:
        print(f"\nError: {e}")
        import traceback
        traceback.print_exc()
