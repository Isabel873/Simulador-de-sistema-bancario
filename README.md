"""
APLICACIÓN DE LISTA ENLAZADA
Sistema de registro y gestión de clientes
"""

# from estructuras.lista import ListaEnlazada

class RegistroClientes:
    """
    Gestiona el registro de clientes usando Lista Enlazada
    
    APLICACIÓN DE LISTA:
    - Almacena clientes en orden de registro
    - Permite búsqueda por diferentes criterios
    - Mantiene orden de inserción
    """
    
    def __init__(self):
        """Inicializa el registro con una lista vacía"""
        # self.clientes = ListaEnlazada()
        self.clientes = []  # Simulación de lista enlazada
    
    def registrar_cliente(self, cliente):
        """
        Registra un nuevo cliente en el sistema
        
        Args:
            cliente (Cliente): Cliente a registrar
            
        Returns:
            bool: True si fue exitoso
        """
        # Verificar que no exista un cliente con el mismo DNI
        if self.buscar_por_dni(cliente.dni):
            raise ValueError(f"Ya existe un cliente con DNI {cliente.dni}")
        
        # self.clientes.agregar(cliente)
        self.clientes.append(cliente)
        
        print(f"✓ Cliente registrado: {cliente}")
        return True
    
    def buscar_por_dni(self, dni):
        """
        Busca un cliente por DNI
        
        Args:
            dni (str): DNI del cliente
            
        Returns:
            Cliente: Cliente encontrado o None
        """
        # return self.clientes.buscar(lambda c: c.dni == dni)
        for cliente in self.clientes:
            if cliente.dni == dni:
                return cliente
        return None
    
    def buscar_por_id(self, id_cliente):
        """
        Busca un cliente por ID
        
        Args:
            id_cliente (int): ID del cliente
            
        Returns:
            Cliente: Cliente encontrado o None
        """
        # return self.clientes.buscar(lambda c: c.id_cliente == id_cliente)
        for cliente in self.clientes:
            if cliente.id_cliente == id_cliente:
                return cliente
        return None
    
    def buscar_por_nombre(self, nombre):
        """
        Busca clientes por nombre (coincidencia parcial)
        
        Args:
            nombre (str): Nombre a buscar
            
        Returns:
            list: Lista de clientes encontrados
        """
        nombre_lower = nombre.lower()
        # clientes_lista = self.clientes.listar_todos()
        
        return [
            c for c in self.clientes
            if nombre_lower in c.nombre.lower() or nombre_lower in c.apellido.lower()
        ]
    
    def buscar_por_email(self, email):
        """
        Busca un cliente por email
        
        Args:
            email (str): Email del cliente
            
        Returns:
            Cliente: Cliente encontrado o None
        """
        # return self.clientes.buscar(lambda c: c.email == email)
        for cliente in self.clientes:
            if cliente.email == email:
                return cliente
        return None
    
    def eliminar_cliente(self, dni):
        """
        Elimina un cliente del registro
        
        Args:
            dni (str): DNI del cliente a eliminar
            
        Returns:
            bool: True si fue exitoso
        """
        cliente = self.buscar_por_dni(dni)
        
        if not cliente:
            raise ValueError(f"No se encontró cliente con DNI {dni}")
        
        # Verificar que no tenga cuentas activas
        if cliente.obtener_total_cuentas() > 0:
            raise Exception("No se puede eliminar un cliente con cuentas activas")
        
        # return self.clientes.eliminar(lambda c: c.dni == dni)
        self.clientes.remove(cliente)
        print(f"✓ Cliente eliminado: {cliente}")
        return True
    
    def listar_todos(self):
        """
        Lista todos los clientes registrados
        
        Returns:
            list: Lista de todos los clientes
        """
        # return self.clientes.listar_todos()
        return self.clientes.copy()
    
    def listar_activos(self):
        """
        Lista solo clientes activos
        
        Returns:
            list: Lista de clientes activos
        """
        # clientes_lista = self.clientes.listar_todos()
        return [c for c in self.clientes if c.activo]
    
    def total_clientes(self):
        """
        Retorna el número total de clientes
        
        Returns:
            int: Cantidad de clientes
        """
        # return self.clientes.tamano()
        return len(self.clientes)
    
    def total_activos(self):
        """
        Retorna el número de clientes activos
        
        Returns:
            int: Cantidad de clientes activos
        """
        return len(self.listar_activos())
    
    def obtener_estadisticas(self):
        """
        Genera estadísticas del registro
        
        Returns:
            dict: Estadísticas del registro
        """
        total = self.total_clientes()
        activos = self.total_activos()
        
        # Calcular saldo total del banco
        saldo_total = sum(c.obtener_saldo_total() for c in self.clientes)
        
        return {
            'total_clientes': total,
            'clientes_activos': activos,
            'clientes_inactivos': total - activos,
            'saldo_total_banco': saldo_total
        }
    
    def generar_reporte(self):
        """Genera un reporte detallado de todos los clientes"""
        print("\n" + "="*70)
        print("REPORTE DE CLIENTES")
        print("="*70)
        
        stats = self.obtener_estadisticas()
        print(f"Total de clientes: {stats['total_clientes']}")
        print(f"Clientes activos: {stats['clientes_activos']}")
        print(f"Saldo total en el banco: ${stats['saldo_total_banco']:.2f}")
        print("\nDetalle de clientes:")
        print("-" * 70)
        
        for cliente in self.clientes:
            print(f"\n{cliente}")
            print(f"  Email: {cliente.email}")
            print(f"  Cuentas: {cliente.obtener_total_cuentas()}")
            print(f"  Saldo total: ${cliente.obtener_saldo_total():.2f}")
            print(f"  Estado: {'ACTIVO' if cliente.activo else 'INACTIVO'}")
        
        print("\n" + "="*70 + "\n")
    
    def __str__(self):
        return f"RegistroClientes(total={self.total_clientes()}, activos={self.total_activos()})"