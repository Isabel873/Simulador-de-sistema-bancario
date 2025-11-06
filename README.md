"""
APLICACIÓN DE COLA (FIFO)
Sistema de procesamiento de transacciones bancarias
Las transacciones se procesan en orden de llegada
"""

from datetime import datetime

# from estructuras.cola import Cola

class Transaccion:
    """
    Representa una transacción bancaria individual
    """
    
    contador_transacciones = 0
    
    def __init__(self, tipo, monto, cuenta_origen, cuenta_destino=None, descripcion=""):
        """
        Inicializa una transacción
        
        Args:
            tipo (str): DEPOSITO, RETIRO, TRANSFERENCIA
            monto (float): Monto de la transacción
            cuenta_origen (Cuenta): Cuenta que origina la transacción
            cuenta_destino (Cuenta): Cuenta destino (para transferencias)
            descripcion (str): Descripción adicional
        """
        Transaccion.contador_transacciones += 1
        self.id_transaccion = Transaccion.contador_transacciones
        self.tipo = tipo
        self.monto = monto
        self.cuenta_origen = cuenta_origen
        self.cuenta_destino = cuenta_destino
        self.descripcion = descripcion
        self.fecha = datetime.now()
        self.estado = "PENDIENTE"  # PENDIENTE, PROCESADA, FALLIDA
        self.mensaje_error = None
    
    def procesar(self):
        """
        Ejecuta la transacción
        
        Returns:
            bool: True si fue exitosa
        """
        try:
            if self.tipo == "DEPOSITO":
                self.cuenta_origen.depositar(self.monto, self.descripcion)
                
            elif self.tipo == "RETIRO":
                self.cuenta_origen.retirar(self.monto, self.descripcion)
                
            elif self.tipo == "TRANSFERENCIA":
                if self.cuenta_destino is None:
                    raise ValueError("Se requiere cuenta destino para transferencias")
                self.cuenta_origen.retirar(self.monto, f"Transferencia a cuenta {self.cuenta_destino.numero_cuenta}")
                self.cuenta_destino.depositar(self.monto, f"Transferencia desde cuenta {self.cuenta_origen.numero_cuenta}")
            
            self.estado = "PROCESADA"
            return True
            
        except Exception as e:
            self.estado = "FALLIDA"
            self.mensaje_error = str(e)
            return False
    
    def __str__(self):
        return f"Transacción #{self.id_transaccion} - {self.tipo}: ${self.monto:.2f} [{self.estado}]"


class SistemaTransacciones:
    """
    Gestiona la cola de transacciones pendientes usando Cola FIFO
    
    APLICACIÓN DE COLA:
    - Las transacciones se encolan cuando se reciben
    - Se procesan en orden de llegada (FIFO)
    - Simula el sistema de un cajero automático o ventanilla
    """
    
    def __init__(self):
        """Inicializa el sistema con una cola vacía"""
        # self.cola_pendientes = Cola()  # Cola FIFO para transacciones
        self.cola_pendientes = []  # Simulación de cola
        self.transacciones_procesadas = []
        self.transacciones_fallidas = []
    
    def agregar_transaccion(self, transaccion):
        """
        Agrega una transacción a la cola de pendientes
        
        Args:
            transaccion (Transaccion): Transacción a encolar
            
        Returns:
            int: ID de la transacción
        """
        # self.cola_pendientes.encolar(transaccion)
        self.cola_pendientes.append(transaccion)
        print(f"✓ Transacción #{transaccion.id_transaccion} agregada a la cola")
        print(f"  Transacciones en cola: {len(self.cola_pendientes)}")
        return transaccion.id_transaccion
    
    def procesar_siguiente(self):
        """
        Procesa la siguiente transacción en la cola (FIFO)
        
        Returns:
            Transaccion: Transacción procesada o None
        """
        # if self.cola_pendientes.esta_vacia():
        if not self.cola_pendientes:
            print("⚠ No hay transacciones pendientes")
            return None
        
        # Desencolar la primera transacción (FIFO)
        # transaccion = self.cola_pendientes.desencolar()
        transaccion = self.cola_pendientes.pop(0)
        
        print(f"\n⏳ Procesando: {transaccion}")
        
        # Procesar la transacción
        exitosa = transaccion.procesar()
        
        if exitosa:
            self.transacciones_procesadas.append(transaccion)
            print(f"✓ Transacción #{transaccion.id_transaccion} procesada exitosamente")
        else:
            self.transacciones_fallidas.append(transaccion)
            print(f"✗ Transacción #{transaccion.id_transaccion} fallida: {transaccion.mensaje_error}")
        
        return transaccion
    
    def procesar_todas(self):
        """
        Procesa todas las transacciones pendientes en orden FIFO
        
        Returns:
            dict: Estadísticas del procesamiento
        """
        print("\n" + "="*60)
        print("PROCESANDO TODAS LAS TRANSACCIONES EN COLA")
        print("="*60)
        
        total = len(self.cola_pendientes)
        procesadas = 0
        
        # while not self.cola_pendientes.esta_vacia():
        while self.cola_pendientes:
            self.procesar_siguiente()
            procesadas += 1
        
        print("\n" + "="*60)
        print(f"PROCESAMIENTO COMPLETADO")
        print(f"Total procesadas: {procesadas}")
        print(f"Exitosas: {len(self.transacciones_procesadas)}")
        print(f"Fallidas: {len(self.transacciones_fallidas)}")
        print("="*60 + "\n")
        
        return {
            'total': total,
            'exitosas': len(self.transacciones_procesadas),
            'fallidas': len(self.transacciones_fallidas)
        }
    
    def obtener_transacciones_pendientes(self):
        """Retorna lista de transacciones pendientes"""
        return self.cola_pendientes.copy() if isinstance(self.cola_pendientes, list) else []
    
    def obtener_estadisticas(self):
        """Retorna estadísticas del sistema"""
        return {
            'pendientes': len(self.cola_pendientes),
            'procesadas': len(self.transacciones_procesadas),
            'fallidas': len(self.transacciones_fallidas),
            'total': Transaccion.contador_transacciones
        }
    
    def limpiar_historial(self):
        """Limpia las transacciones procesadas y fallidas"""
        self.transacciones_procesadas = []
        self.transacciones_fallidas = []
        return True
    
    def __str__(self):
        stats = self.obtener_estadisticas()
        return f"SistemaTransacciones(pendientes={stats['pendientes']}, procesadas={stats['procesadas']})"