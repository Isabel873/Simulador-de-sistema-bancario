"""
CLASE PRINCIPAL - BANCO
Integra todas las estructuras de datos y modelos
"""

from datetime import datetime

# Importaciones (en proyecto real desde módulos correspondientes)
# from modelos.cliente import Cliente
# from modelos.cuenta_ahorro import CuentaAhorro
# from modelos.cuenta_corriente import CuentaCorriente
# from servicios.registro_clientes import RegistroClientes
# from servicios.sistema_transacciones import SistemaTransacciones, Transaccion
# from servicios.historial_operaciones import HistorialOperaciones, Operacion
# from estructuras.arbol_binario import ArbolBinarioBusqueda
# from estructuras.array_dinamico import ArrayDinamico


class Banco:
    """
    Clase principal que coordina todo el sistema bancario
    
    INTEGRA TODAS LAS ESTRUCTURAS:
    - Lista Enlazada: Registro de clientes
    - Árbol Binario: Índice de cuentas
    - Cola FIFO: Transacciones pendientes
    - Pila LIFO: Historial de operaciones
    - Array Dinámico: Log de actividades
    """
    
    def __init__(self, nombre_banco):
        """
        Inicializa el sistema bancario completo
        
        Args:
            nombre_banco (str): Nombre del banco
        """
        self.nombre = nombre_banco
        self.fecha_fundacion = datetime.now()
        
        # Inicializar componentes (usando estructuras de datos)
        # self.registro_clientes = RegistroClientes()  # Lista Enlazada
        # self.indice_cuentas = ArbolBinarioBusqueda()  # BST
        # self.sistema_transacciones = SistemaTransacciones()  # Cola FIFO
        # self.historial = HistorialOperaciones()  # Pila LIFO
        # self.log_actividades = ArrayDinamico(50)  # Array Dinámico
        
        # Simulación para el ejemplo
        self.registro_clientes = []
        self.indice_cuentas = {}
        self.sistema_transacciones = []
        self.historial = []
        self.log_actividades = []
        
        self._log(f"Banco '{nombre_banco}' inicializado")
    
    def _log(self, mensaje):
        """
        Registra una actividad en el log (Array Dinámico)
        
        Args:
            mensaje (str): Mensaje a registrar
        """
        entrada = {
            'timestamp': datetime.now(),
            'mensaje': mensaje
        }
        # self.log_actividades.agregar(entrada)
        self.log_actividades.append(entrada)
    
    # ========== GESTIÓN DE CLIENTES ==========
    
    def registrar_cliente(self, nombre, apellido, dni, email, telefono="", direccion=""):
        """
        Registra un nuevo cliente en el banco
        
        Args:
            nombre, apellido, dni, email, telefono, direccion
            
        Returns:
            Cliente: Cliente registrado
        """
        # cliente = Cliente(nombre, apellido, dni, email, telefono, direccion)
        # self.registro_clientes.registrar_cliente(cliente)
        
        cliente = {
            'id': len(self.registro_clientes) + 1,
            'nombre': nombre,
            'apellido': apellido,
            'dni': dni,
            'email': email,
            'cuentas': []
        }
        self.registro_clientes.append(cliente)
        
        self._log(f"Cliente registrado: {nombre} {apellido} (DNI: {dni})")
        return cliente
    
    def buscar_cliente(self, dni):
        """Busca un cliente por DNI"""
        # return self.registro_clientes.buscar_por_dni(dni)
        for cliente in self.registro_clientes:
            if cliente['dni'] == dni:
                return cliente
        return None
    
    # ========== GESTIÓN DE CUENTAS ==========
    
    def crear_cuenta_ahorro(self, dni_cliente, saldo_inicial=0, tasa_interes=0.02):
        """
        Crea una cuenta de ahorro para un cliente
        
        Args:
            dni_cliente (str): DNI del cliente
            saldo_inicial (float): Saldo inicial
            tasa_interes (float): Tasa de interés anual
            
        Returns:
            CuentaAhorro: Cuenta creada
        """
        cliente = self.buscar_cliente(dni_cliente)
        
        if not cliente:
            raise ValueError(f"No se encontró cliente con DNI {dni_cliente}")
        
        # cuenta = CuentaAhorro(cliente, saldo_inicial, tasa_interes)
        # cliente.agregar_cuenta(cuenta)
        # self.indice_cuentas.insertar(cuenta.numero_cuenta, cuenta)
        
        numero_cuenta = len(self.indice_cuentas) + 1001
        cuenta = {
            'numero': numero_cuenta,
            'tipo': 'AHORRO',
            'cliente': cliente,
            'saldo': saldo_inicial,
            'tasa_interes': tasa_interes
        }
        self.indice_cuentas[numero_cuenta] = cuenta
        cliente['cuentas'].append(cuenta)
        
        self._log(f"Cuenta Ahorro #{numero_cuenta} creada para {cliente['nombre']} {cliente['apellido']}")
        return cuenta
    
    def crear_cuenta_corriente(self, dni_cliente, saldo_inicial=0, limite_sobregiro=1000):
        """
        Crea una cuenta corriente para un cliente
        
        Args:
            dni_cliente (str): DNI del cliente
            saldo_inicial (float): Saldo inicial
            limite_sobregiro (float): Límite de sobregiro
            
        Returns:
            CuentaCorriente: Cuenta creada
        """
        cliente = self.buscar_cliente(dni_cliente)
        
        if not cliente:
            raise ValueError(f"No se encontró cliente con DNI {dni_cliente}")
        
        numero_cuenta = len(self.indice_cuentas) + 1001
        cuenta = {
            'numero': numero_cuenta,
            'tipo': 'CORRIENTE',
            'cliente': cliente,
            'saldo': saldo_inicial,
            'limite_sobregiro': limite_sobregiro
        }
        self.indice_cuentas[numero_cuenta] = cuenta
        cliente['cuentas'].append(cuenta)
        
        self._log(f"Cuenta Corriente #{numero_cuenta} creada para {cliente['nombre']} {cliente['apellido']}")
        return cuenta
    
    def buscar_cuenta(self, numero_cuenta):
        """
        Busca una cuenta por número usando Árbol Binario (O(log n))
        
        Args:
            numero_cuenta (int): Número de cuenta
            
        Returns:
            Cuenta: Cuenta encontrada o None
        """
        # return self.indice_cuentas.buscar(numero_cuenta)
        return self.indice_cuentas.get(numero_cuenta)
    
    # ========== OPERACIONES BANCARIAS ==========
    
    def depositar(self, numero_cuenta, monto):
        """
        Realiza un depósito en una cuenta
        
        Args:
            numero_cuenta (int): Número de cuenta
            monto (float): Monto a depositar
            
        Returns:
            bool: True si fue exitoso
        """
        cuenta = self.buscar_cuenta(numero_cuenta)
        
        if not cuenta:
            raise ValueError(f"Cuenta {numero_cuenta} no encontrada")
        
        # Guardar estado para historial
        saldo_anterior = cuenta['saldo']
        
        # Realizar depósito
        cuenta['saldo'] += monto
        
        # Registrar en historial (Pila LIFO)
        # operacion = Operacion("DEPOSITO", cuenta, saldo_anterior, cuenta.saldo)
        # self.historial.registrar_operacion(operacion)
        
        # Agregar a cola de transacciones
        # transaccion = Transaccion("DEPOSITO", monto, cuenta)
        # self.sistema_transacciones.agregar_transaccion(transaccion)
        
        self._log(f"Depósito ${monto:.2f} en cuenta #{numero_cuenta}")
        return True
    
    def retirar(self, numero_cuenta, monto):
        """
        Realiza un retiro de una cuenta
        
        Args:
            numero_cuenta (int): Número de cuenta
            monto (float): Monto a retirar
            
        Returns:
            bool: True si fue exitoso
        """
        cuenta = self.buscar_cuenta(numero_cuenta)
        
        if not cuenta:
            raise ValueError(f"Cuenta {numero_cuenta} no encontrada")
        
        if cuenta['saldo'] < monto:
            raise ValueError("Saldo insuficiente")
        
        saldo_anterior = cuenta['saldo']
        cuenta['saldo'] -= monto
        
        self._log(f"Retiro ${monto:.2f} de cuenta #{numero_cuenta}")
        return True
    
    def transferir(self, numero_cuenta_origen, numero_cuenta_destino, monto):
        """
        Realiza una transferencia entre cuentas
        
        Args:
            numero_cuenta_origen (int): Cuenta origen
            numero_cuenta_destino (int): Cuenta destino
            monto (float): Monto a transferir
            
        Returns:
            bool: True si fue exitoso
        """
        cuenta_origen = self.buscar_cuenta(numero_cuenta_origen)
        cuenta_destino = self.buscar_cuenta(numero_cuenta_destino)
        
        if not cuenta_origen or not cuenta_destino:
            raise ValueError("Una o ambas cuentas no existen")
        
        if cuenta_origen['saldo'] < monto:
            raise ValueError("Saldo insuficiente en cuenta origen")
        
        # Realizar transferencia
        cuenta_origen['saldo'] -= monto
        cuenta_destino['saldo'] += monto
        
        self._log(f"Transferencia ${monto:.2f}: #{numero_cuenta_origen} → #{numero_cuenta_destino}")
        return True
    
    # ========== REPORTES Y ESTADÍSTICAS ==========
    
    def generar_reporte_general(self):
        """Genera un reporte completo del banco"""
        print("\n" + "="*70)
        print(f"BANCO {self.nombre.upper()} - REPORTE GENERAL")
        print("="*70)
        print(f"Fecha: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")
        print(f"\nTotal de clientes: {len(self.registro_clientes)}")
        print(f"Total de cuentas: {len(self.indice_cuentas)}")
        
        saldo_total = sum(c['saldo'] for c in self.indice_cuentas.values())
        print(f"Saldo total en el banco: ${saldo_total:.2f}")
        
        print("\nActividad reciente:")
        for log in self.log_actividades[-5:]:
            print(f"  {log['timestamp'].strftime('%H:%M:%S')} - {log['mensaje']}")
        
        print("="*70 + "\n")
    
    def __str__(self):
        return f"Banco(nombre='{self.nombre}', clientes={len(self.registro_clientes)}, cuentas={len(self.indice_cuentas)})"