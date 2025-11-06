"""
PROGRAMACIÓN ORIENTADA A OBJETOS - HERENCIA
Clase base Cuenta (Clase padre)
"""

from datetime import datetime
from abc import ABC, abstractmethod

class Cuenta(ABC):
    "Clase abstracta base para cuentas bancarias Define la interfaz común para todos los tipos de cuenta."
    
    "Esta es la CLASE PADRE en la jerarquía de herencia."
    
    # Contador de cuentas (variable de clase compartida)
    contador_cuentas = 1000
    
    def __init__(self, cliente, saldo_inicial=0.0):
        """
        Inicializa una cuenta bancaria
        
        Args:
            cliente (Cliente): Cliente propietario de la cuenta
            saldo_inicial (float): Saldo inicial de la cuenta
        """
        Cuenta.contador_cuentas += 1
        self.numero_cuenta = Cuenta.contador_cuentas
        self.cliente = cliente
        self.saldo = saldo_inicial
        self.fecha_apertura = datetime.now()
        self.activa = True
        self.historial_transacciones = []
    
    def depositar(self, monto, descripcion="Depósito"):
        """Deposita dinero en la cuenta
        
        Args:
            monto (float): Cantidad a depositar
            descripcion (str): Descripción de la transacción
            
        Returns:
            bool: True si la operación fue exitosa
        """
        if monto <= 0:
            raise ValueError("El monto debe ser mayor a 0")
        
        if not self.activa:
            raise Exception("La cuenta está inactiva")
        
        self.saldo += monto
        self._registrar_transaccion("DEPOSITO", monto, descripcion)
        return True
    
    def retirar(self, monto, descripcion="Retiro"):
        """
        Retira dinero de la cuenta
        
        Args:
            monto (float): Cantidad a retirar
            descripcion (str): Descripción de la transacción
            
        Returns:
            bool: True si la operación fue exitosa
        """
        if monto <= 0:
            raise ValueError("El monto debe ser mayor a 0")
        
        if not self.activa:
            raise Exception("La cuenta está inactiva")
        
        if self.saldo < monto:
            raise ValueError("Saldo insuficiente")
        
        self.saldo -= monto
        self._registrar_transaccion("RETIRO", monto, descripcion)
        return True
    
    def _registrar_transaccion(self, tipo, monto, descripcion):
        """
        Registra una transacción en el historial (método privado)
        
        Args:
            tipo (str): Tipo de transacción (DEPOSITO, RETIRO, etc.)
            monto (float): Monto de la transacción
            descripcion (str): Descripción de la transacción
        """
        transaccion = {
            'tipo': tipo,
            'monto': monto,
            'descripcion': descripcion,
            'fecha': datetime.now(),
            'saldo_resultante': self.saldo
        }
        self.historial_transacciones.append(transaccion)
    
    def consultar_saldo(self):
        """Retorna el saldo actual de la cuenta"""
        return self.saldo
    
    def obtener_historial(self, ultimas_n=None):
        """
        Obtiene el historial de transacciones
        
        Args:
            ultimas_n (int): Número de transacciones recientes a mostrar
            
        Returns:
            list: Lista de transacciones
        """
        if ultimas_n:
            return self.historial_transacciones[-ultimas_n:]
        return self.historial_transacciones.copy()
    
    def desactivar(self):
        """Desactiva la cuenta"""
        self.activa = False
        return True
    
    def activar(self):
        """Activa la cuenta"""
        self.activa = True
        return True
    
    @abstractmethod
    def obtener_tipo_cuenta(self):
        """
        Método abstracto: debe ser implementado por las clases hijas
        Retorna el tipo de cuenta
        """
        pass
    
    @abstractmethod
    def calcular_mantenimiento(self):
        """
        Método abstracto: debe ser implementado por las clases hijas
        Calcula el costo de mantenimiento de la cuenta
        """
        pass
    
    def obtener_info(self):
        """Retorna información resumida de la cuenta"""
        return {
            'numero_cuenta': self.numero_cuenta,
            'tipo': self.obtener_tipo_cuenta(),
            'cliente': self.cliente.obtener_nombre_completo(),
            'saldo': self.saldo,
            'fecha_apertura': self.fecha_apertura.strftime("%Y-%m-%d"),
            'activa': self.activa,
            'total_transacciones': len(self.historial_transacciones)
        }
    
    def __str__(self):
        """Representación en string de la cuenta"""
        return f"Cuenta {self.numero_cuenta} ({self.obtener_tipo_cuenta()}) - Saldo: ${self.saldo:.2f}"
    
    def __repr__(self):
        """Representación técnica de la cuenta"""
        return f"Cuenta(numero={self.numero_cuenta}, tipo='{self.obtener_tipo_cuenta()}', saldo={self.saldo})"
    
    def __eq__(self, otra):
        """Comparación de igualdad entre cuentas"""
        if isinstance(otra, Cuenta):
            return self.numero_cuenta == otra.numero_cuenta
        return False
    
    def __hash__(self):
        """Hash de la cuenta basado en número de cuenta"""
        return hash(self.numero_cuenta)