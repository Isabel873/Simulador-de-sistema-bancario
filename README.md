"""
APLICACIÓN DE PILA (LIFO)
Sistema de deshacer/rehacer operaciones bancarias
Las operaciones se deshacen en orden inverso (última primero)
"""

from datetime import datetime

# from estructuras.pila import Pila

class Operacion:
    """
    Representa una operación reversible en el sistema
    """
    
    def __init__(self, tipo, cuenta, monto_anterior, monto_nuevo, descripcion=""):
        """
        Inicializa una operación
        
        Args:
            tipo (str): Tipo de operación
            cuenta (Cuenta): Cuenta afectada
            monto_anterior (float): Saldo antes de la operación
            monto_nuevo (float): Saldo después de la operación
            descripcion (str): Descripción de la operación
        """
        self.tipo = tipo
        self.cuenta = cuenta
        self.monto_anterior = monto_anterior
        self.monto_nuevo = monto_nuevo
        self.descripcion = descripcion
        self.fecha = datetime.now()
    
    def revertir(self):
        """Revierte la operación"""
        self.cuenta.saldo = self.monto_anterior
        return True
    
    def reaplicar(self):
        """Reaplica la operación"""
        self.cuenta.saldo = self.monto_nuevo
        return True
    
    def __str__(self):
        return f"{self.tipo}: ${self.monto_anterior:.2f} → ${self.monto_nuevo:.2f}"


class HistorialOperaciones:
    """
    Gestiona el historial de operaciones con funcionalidad de deshacer/rehacer
    
    APLICACIÓN DE PILA LIFO:
    - pila_deshacer: Guarda operaciones realizadas
    - pila_rehacer: Guarda operaciones deshechas
    - Deshacer: Saca de pila_deshacer (última operación)
    - Rehacer: Saca de pila_rehacer (última operación deshecha)
    """
    
    def __init__(self):
        """Inicializa el historial con dos pilas"""
        # self.pila_deshacer = Pila()  # Pila LIFO para operaciones
        # self.pila_rehacer = Pila()   # Pila LIFO para rehacer
        self.pila_deshacer = []  # Simulación de pila
        self.pila_rehacer = []   # Simulación de pila
    
    def registrar_operacion(self, operacion):
        """
        Registra una nueva operación en la pila de deshacer
        
        Args:
            operacion (Operacion): Operación a registrar
        """
        # self.pila_deshacer.apilar(operacion)
        self.pila_deshacer.append(operacion)
        
        # Al hacer una nueva operación, limpiar pila de rehacer
        # self.pila_rehacer.limpiar()
        self.pila_rehacer = []
        
        print(f"✓ Operación registrada: {operacion}")
    
    def deshacer(self):
        """
        Deshace la última operación realizada (LIFO)
        
        Returns:
            Operacion: Operación deshecha o None
        """
        # if self.pila_deshacer.esta_vacia():
        if not self.pila_deshacer:
            print("⚠ No hay operaciones para deshacer")
            return None
        
        # Desapilar la última operación (LIFO)
        # operacion = self.pila_deshacer.desapilar()
        operacion = self.pila_deshacer.pop()
        
        # Revertir la operación
        operacion.revertir()
        
        # Mover a pila de rehacer
        # self.pila_rehacer.apilar(operacion)
        self.pila_rehacer.append(operacion)
        
        print(f"↶ Operación deshecha: {operacion}")
        print(f"  Nuevo saldo: ${operacion.cuenta.saldo:.2f}")
        
        return operacion
    
    def rehacer(self):
        """
        Rehace la última operación deshecha (LIFO)
        
        Returns:
            Operacion: Operación rehecha o None
        """
        # if self.pila_rehacer.esta_vacia():
        if not self.pila_rehacer:
            print("⚠ No hay operaciones para rehacer")
            return None
        
        # Desapilar de pila rehacer (LIFO)
        # operacion = self.pila_rehacer.desapilar()
        operacion = self.pila_rehacer.pop()
        
        # Reaplicar la operación
        operacion.reaplicar()
        
        # Mover de vuelta a pila de deshacer
        # self.pila_deshacer.apilar(operacion)
        self.pila_deshacer.append(operacion)
        
        print(f"↷ Operación rehecha: {operacion}")
        print(f"  Nuevo saldo: ${operacion.cuenta.saldo:.2f}")
        
        return operacion
    
    def puede_deshacer(self):
        """Verifica si hay operaciones para deshacer"""
        # return not self.pila_deshacer.esta_vacia()
        return len(self.pila_deshacer) > 0
    
    def puede_rehacer(self):
        """Verifica si hay operaciones para rehacer"""
        # return not self.pila_rehacer.esta_vacia()
        return len(self.pila_rehacer) > 0
    
    def obtener_historial(self, ultimas_n=10):
        """
        Obtiene las últimas N operaciones
        
        Args:
            ultimas_n (int): Número de operaciones a mostrar
            
        Returns:
            list: Lista de operaciones
        """
        # operaciones = self.pila_deshacer.items[-ultimas_n:]
        operaciones = self.pila_deshacer[-ultimas_n:]
        return operaciones
    
    def limpiar(self):
        """Limpia ambas pilas"""
        # self.pila_deshacer.limpiar()
        # self.pila_rehacer.limpiar()
        self.pila_deshacer = []
        self.pila_rehacer = []
        print("✓ Historial limpiado")
        return True
    
    def obtener_estadisticas(self):
        """Retorna estadísticas del historial"""
        return {
            'operaciones_realizadas': len(self.pila_deshacer),
            'operaciones_deshechas': len(self.pila_rehacer),
            'puede_deshacer': self.puede_deshacer(),
            'puede_rehacer': self.puede_rehacer()
        }
    
    def __str__(self):
        stats = self.obtener_estadisticas()
        return f"HistorialOperaciones(realizadas={stats['operaciones_realizadas']}, deshechas={stats['operaciones_deshechas']})"