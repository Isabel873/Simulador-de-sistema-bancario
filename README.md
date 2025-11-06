"""
Clase Cliente - Modelo de datos principal
"""

from datetime import datetime

class Cliente:
    """
    Representa un cliente del banco.
    
    Atributos:
        id_cliente (int): Identificador único del cliente
        nombre (str): Nombre del cliente
        apellido (str): Apellido del cliente
        dni (str): Documento de identidad
        email (str): Correo electrónico
        telefono (str): Número de teléfono
        direccion (str): Dirección del cliente
        fecha_registro (datetime): Fecha de registro en el banco
        cuentas (list): Lista de cuentas del cliente
        activo (bool): Estado del cliente
    """
    
    # Contador de clientes (variable de clase)
    contador_clientes = 0
    
    def __init__(self, nombre, apellido, dni, email, telefono="", direccion=""):
        """
        Inicializa un nuevo cliente
        
        Args:
            nombre (str): Nombre del cliente
            apellido (str): Apellido del cliente
            dni (str): Documento de identidad
            email (str): Correo electrónico
            telefono (str): Número de teléfono (opcional)
            direccion (str): Dirección (opcional)
        """
        Cliente.contador_clientes += 1
        self.id_cliente = Cliente.contador_clientes
        self.nombre = nombre
        self.apellido = apellido
        self.dni = dni
        self.email = email
        self.telefono = telefono
        self.direccion = direccion
        self.fecha_registro = datetime.now()
        self.cuentas = []  # Lista de cuentas asociadas
        self.activo = True
    
    def agregar_cuenta(self, cuenta):
        """
        Asocia una cuenta bancaria al cliente
        
        Args:
            cuenta (Cuenta): Objeto de tipo Cuenta
        """
        if cuenta not in self.cuentas:
            self.cuentas.append(cuenta)
            return True
        return False
    
    def obtener_nombre_completo(self):
        """Retorna el nombre completo del cliente"""
        return f"{self.nombre} {self.apellido}"
    
    def obtener_total_cuentas(self):
        """Retorna el número de cuentas del cliente"""
        return len(self.cuentas)
    
    def obtener_saldo_total(self):
        """
        Calcula el saldo total sumando todas las cuentas
        
        Returns:
            float: Saldo total de todas las cuentas
        """
        return sum(cuenta.saldo for cuenta in self.cuentas)
    
    def desactivar(self):
        """Desactiva el cliente en el sistema"""
        self.activo = False
        return True
    
    def activar(self):
        """Activa el cliente en el sistema"""
        self.activo = True
        return True
    
    def actualizar_email(self, nuevo_email):
        """Actualiza el correo electrónico del cliente"""
        self.email = nuevo_email
        return True
    
    def actualizar_telefono(self, nuevo_telefono):
        """Actualiza el teléfono del cliente"""
        self.telefono = nuevo_telefono
        return True
    
    def actualizar_direccion(self, nueva_direccion):
        """Actualiza la dirección del cliente"""
        self.direccion = nueva_direccion
        return True
    
    def obtener_info(self):
        """
        Retorna un diccionario con la información del cliente
        
        Returns:
            dict: Información completa del cliente
        """
        return {
            'id': self.id_cliente,
            'nombre_completo': self.obtener_nombre_completo(),
            'dni': self.dni,
            'email': self.email,
            'telefono': self.telefono,
            'direccion': self.direccion,
            'fecha_registro': self.fecha_registro.strftime("%Y-%m-%d"),
            'total_cuentas': self.obtener_total_cuentas(),
            'saldo_total': self.obtener_saldo_total(),
            'activo': self.activo
        }
    
    def __str__(self):
        """Representación en string del cliente"""
        return f"Cliente #{self.id_cliente}: {self.obtener_nombre_completo()} (DNI: {self.dni})"
    
    def __repr__(self):
        """Representación técnica del cliente"""
        return f"Cliente(id={self.id_cliente}, nombre='{self.nombre}', apellido='{self.apellido}', dni='{self.dni}')"
    
    def __eq__(self, otro):
        """Comparación de igualdad entre clientes (por DNI)"""
        if isinstance(otro, Cliente):
            return self.dni == otro.dni
        return False
    
    def __hash__(self):
        """Hash del cliente basado en DNI"""
        return hash(self.dni)