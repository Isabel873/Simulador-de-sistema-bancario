"""
HERENCIA Y POLIMORFISMO
CuentaAhorro - Clase hija que hereda de Cuenta
"""

from datetime import datetime

# Importar la clase padre (en tu proyecto real sería: from modelos.cuenta import Cuenta)
# from cuenta import Cuenta

class CuentaAhorro:  # En el código real: class CuentaAhorro(Cuenta):
    """
    Cuenta de Ahorro con tasa de interés.
    
    HERENCIA: Esta clase HEREDA de Cuenta (clase padre)
    Extiende funcionalidad con: tasa de interés y límite de retiros mensuales
    
    POLIMORFISMO: Sobrescribe métodos de la clase padre
    """
    
    def __init__(self, cliente, saldo_inicial=0.0, tasa_interes=0.02, limite_retiros=5):
        """
        Inicializa una cuenta de ahorro
        
        Args:
            cliente (Cliente): Cliente propietario
            saldo_inicial (float): Saldo inicial
            tasa_interes (float): Tasa de interés anual (default 2%)
            limite_retiros (int): Límite de retiros mensuales
        """
        # super().__init__(cliente, saldo_inicial)  # Llama al constructor padre
        
        # Atributos específicos de CuentaAhorro
        self.tasa_interes = tasa_interes
        self.limite_retiros_mensual = limite_retiros
        self.retiros_realizados = 0
        self.mes_actual = datetime.now().month
    
    def calcular_interes(self):
        """
        Calcula y aplica el interés al saldo
        Método específico de CuentaAhorro (no existe en clase padre)
        
        Returns:
            float: Monto del interés generado
        """
        interes = self.saldo * self.tasa_interes
        self.depositar(interes, f"Interés generado ({self.tasa_interes*100}%)")
        return interes
    
    def retirar(self, monto, descripcion="Retiro"):
        """
        POLIMORFISMO: Sobrescribe el método retirar() de la clase padre
        Agrega validación de límite de retiros mensuales
        
        Args:
            monto (float): Cantidad a retirar
            descripcion (str): Descripción
            
        Returns:
            bool: True si fue exitoso
        """
        # Verificar si cambió el mes (resetear contador)
        if datetime.now().month != self.mes_actual:
            self.retiros_realizados = 0
            self.mes_actual = datetime.now().month
        
        # Validar límite de retiros
        if self.retiros_realizados >= self.limite_retiros_mensual:
            raise Exception(f"Ha alcanzado el límite de {self.limite_retiros_mensual} retiros mensuales")
        
        # Llamar al método de la clase padre
        # resultado = super().retirar(monto, descripcion)
        # self.retiros_realizados += 1
        # return resultado
        
        # Simulación para el ejemplo:
        if monto > 0 and self.saldo >= monto:
            self.saldo -= monto
            self.retiros_realizados += 1
            return True
        return False
    
    def obtener_tipo_cuenta(self):
        """
        Implementa el método abstracto de la clase padre
        
        Returns:
            str: Tipo de cuenta
        """
        return "AHORRO"
    
    def calcular_mantenimiento(self):
        """
        Implementa el método abstracto de la clase padre
        Cuenta de ahorro no tiene costo de mantenimiento
        
        Returns:
            float: Costo de mantenimiento
        """
        return 0.0
    
    def obtener_retiros_disponibles(self):
        """
        Calcula cuántos retiros quedan disponibles este mes
        
        Returns:
            int: Número de retiros disponibles
        """
        return self.limite_retiros_mensual - self.retiros_realizados
    
    def cambiar_tasa_interes(self, nueva_tasa):
        """
        Actualiza la tasa de interés
        
        Args:
            nueva_tasa (float): Nueva tasa de interés
        """
        if 0 <= nueva_tasa <= 1:
            self.tasa_interes = nueva_tasa
            return True
        raise ValueError("La tasa debe estar entre 0 y 1")
    
    def obtener_info(self):
        """
        Sobrescribe método de la clase padre para incluir info adicional
        
        Returns:
            dict: Información completa de la cuenta
        """
        # info = super().obtener_info()  # Obtener info de la clase padre
        info = {
            'numero_cuenta': self.numero_cuenta,
            'tipo': self.obtener_tipo_cuenta(),
            'saldo': self.saldo
        }
        
        # Agregar información específica de CuentaAhorro
        info['tasa_interes'] = f"{self.tasa_interes*100}%"
        info['retiros_disponibles'] = self.obtener_retiros_disponibles()
        info['limite_retiros_mensual'] = self.limite_retiros_mensual
        
        return info
    
    def __str__(self):
        """Representación personalizada en string"""
        return f"Cuenta Ahorro {self.numero_cuenta} - Saldo: ${self.saldo:.2f} (Interés: {self.tasa_interes*100}%)"