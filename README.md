"""
HERENCIA Y POLIMORFISMO
CuentaCorriente - Otra clase hija que hereda de Cuenta
"""

# from cuenta import Cuenta

class CuentaCorriente:  # En el código real: class CuentaCorriente(Cuenta):
    """
    Cuenta Corriente con sobregiro permitido.
    
    HERENCIA: Esta clase HEREDA de Cuenta (clase padre)
    Extiende funcionalidad con: sobregiro y costo de mantenimiento
    
    POLIMORFISMO: Sobrescribe el método retirar() para permitir sobregiro
    """
    
    def __init__(self, cliente, saldo_inicial=0.0, limite_sobregiro=1000.0, costo_mantenimiento=5.0):
        """
        Inicializa una cuenta corriente
        
        Args:
            cliente (Cliente): Cliente propietario
            saldo_inicial (float): Saldo inicial
            limite_sobregiro (float): Límite de sobregiro permitido
            costo_mantenimiento (float): Costo mensual de mantenimiento
        """
        # super().__init__(cliente, saldo_inicial)
        
        # Atributos específicos de CuentaCorriente
        self.limite_sobregiro = limite_sobregiro
        self.costo_mantenimiento = costo_mantenimiento
        self.sobregiro_utilizado = 0.0
    
    def retirar(self, monto, descripcion="Retiro"):
        """
        POLIMORFISMO: Sobrescribe retirar() para permitir sobregiro
        Permite retirar más dinero del disponible hasta el límite de sobregiro
        
        Args:
            monto (float): Cantidad a retirar
            descripcion (str): Descripción
            
        Returns:
            bool: True si fue exitoso
        """
        if monto <= 0:
            raise ValueError("El monto debe ser mayor a 0")
        
        if not self.activa:
            raise Exception("La cuenta está inactiva")
        
        # Calcular saldo disponible (saldo + límite sobregiro)
        saldo_disponible = self.saldo + self.limite_sobregiro
        
        if monto > saldo_disponible:
            raise ValueError(f"Monto excede el límite de sobregiro. Disponible: ${saldo_disponible:.2f}")
        
        # Realizar retiro
        self.saldo -= monto
        
        # Calcular sobregiro utilizado
        if self.saldo < 0:
            self.sobregiro_utilizado = abs(self.saldo)
        else:
            self.sobregiro_utilizado = 0.0
        
        # self._registrar_transaccion("RETIRO", monto, descripcion)
        return True
    
    def obtener_tipo_cuenta(self):
        """
        Implementa el método abstracto de la clase padre
        
        Returns:
            str: Tipo de cuenta
        """
        return "CORRIENTE"
    
    def calcular_mantenimiento(self):
        """
        Implementa el método abstracto de la clase padre
        Retorna el costo de mantenimiento mensual
        
        Returns:
            float: Costo de mantenimiento
        """
        return self.costo_mantenimiento
    
    def cobrar_mantenimiento(self):
        """
        Cobra el costo de mantenimiento mensual
        Método específico de CuentaCorriente
        
        Returns:
            float: Monto cobrado
        """
        monto = self.calcular_mantenimiento()
        
        if monto > 0:
            self.saldo -= monto
            # self._registrar_transaccion("MANTENIMIENTO", monto, "Cobro mensual de mantenimiento")
        
        return monto
    
    def obtener_saldo_disponible(self):
        """
        Calcula el saldo disponible incluyendo sobregiro
        
        Returns:
            float: Saldo disponible total
        """
        return self.saldo + self.limite_sobregiro
    
    def obtener_sobregiro_disponible(self):
        """
        Calcula cuánto sobregiro queda disponible
        
        Returns:
            float: Sobregiro disponible
        """
        return self.limite_sobregiro - self.sobregiro_utilizado
    
    def esta_en_sobregiro(self):
        """
        Verifica si la cuenta está usando sobregiro
        
        Returns:
            bool: True si está en sobregiro
        """
        return self.saldo < 0
    
    def ajustar_limite_sobregiro(self, nuevo_limite):
        """
        Ajusta el límite de sobregiro
        
        Args:
            nuevo_limite (float): Nuevo límite
            
        Returns:
            bool: True si fue exitoso
        """
        if nuevo_limite >= 0:
            self.limite_sobregiro = nuevo_limite
            return True
        raise ValueError("El límite debe ser mayor o igual a 0")
    
    def obtener_info(self):
        """
        Sobrescribe método de la clase padre para incluir info adicional
        
        Returns:
            dict: Información completa de la cuenta
        """
        # info = super().obtener_info()
        info = {
            'numero_cuenta': self.numero_cuenta,
            'tipo': self.obtener_tipo_cuenta(),
            'saldo': self.saldo
        }
        
        # Agregar información específica de CuentaCorriente
        info['limite_sobregiro'] = self.limite_sobregiro
        info['sobregiro_utilizado'] = self.sobregiro_utilizado
        info['sobregiro_disponible'] = self.obtener_sobregiro_disponible()
        info['saldo_disponible_total'] = self.obtener_saldo_disponible()
        info['costo_mantenimiento'] = self.costo_mantenimiento
        info['en_sobregiro'] = self.esta_en_sobregiro()
        
        return info
    
    def __str__(self):
        """Representación personalizada en string"""
        estado = "EN SOBREGIRO" if self.esta_en_sobregiro() else "NORMAL"
        return f"Cuenta Corriente {self.numero_cuenta} - Saldo: ${self.saldo:.2f} [{estado}]"