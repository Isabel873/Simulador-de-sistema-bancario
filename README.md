"""
ESTRUCTURA DE DATOS: ARRAY DINÁMICO
Aplicación: Almacenar historial de transacciones con redimensionamiento automático
"""

class ArrayDinamico:
    """
    Array que crece automáticamente cuando se llena.
    Similar a la implementación interna de list() en Python.
    """
    
    def __init__(self, capacidad_inicial=10):
        """
        Inicializa el array con una capacidad inicial
        """
        self.capacidad = capacidad_inicial
        self.tamano_actual = 0
        self.array = [None] * self.capacidad
    
    def agregar(self, elemento):
        """
        Agrega un elemento al final del array.
        Si está lleno, duplica la capacidad.
        Complejidad: O(1) amortizado, O(n) cuando redimensiona
        """
        if self.tamano_actual == self.capacidad:
            self._redimensionar()
        
        self.array[self.tamano_actual] = elemento
        self.tamano_actual += 1
        return True
    
    def _redimensionar(self):
        """
        Duplica la capacidad del array cuando se llena
        Complejidad: O(n)
        """
        self.capacidad *= 2
        nuevo_array = [None] * self.capacidad
        
        # Copiar elementos al nuevo array
        for i in range(self.tamano_actual):
            nuevo_array[i] = self.array[i]
        
        self.array = nuevo_array
        print(f"Array redimensionado a capacidad: {self.capacidad}")
    
    def obtener(self, indice):
        """
        Obtiene el elemento en el índice dado
        Complejidad: O(1)
        """
        if 0 <= indice < self.tamano_actual:
            return self.array[indice]
        raise IndexError(f"Índice {indice} fuera de rango")
    
    def actualizar(self, indice, elemento):
        """
        Actualiza el elemento en el índice dado
        Complejidad: O(1)
        """
        if 0 <= indice < self.tamano_actual:
            self.array[indice] = elemento
            return True
        raise IndexError(f"Índice {indice} fuera de rango")
    
    def eliminar(self, indice):
        """
        Elimina el elemento en el índice dado
        Complejidad: O(n)
        """
        if 0 <= indice < self.tamano_actual:
            # Desplazar elementos a la izquierda
            for i in range(indice, self.tamano_actual - 1):
                self.array[i] = self.array[i + 1]
            
            self.array[self.tamano_actual - 1] = None
            self.tamano_actual -= 1
            return True
        raise IndexError(f"Índice {indice} fuera de rango")
    
    def buscar(self, elemento):
        """
        Busca un elemento y retorna su índice
        Complejidad: O(n)
        """
        for i in range(self.tamano_actual):
            if self.array[i] == elemento:
                return i
        return -1
    
    def esta_vacio(self):
        """Verifica si el array está vacío"""
        return self.tamano_actual == 0
    
    def tamano(self):
        """Retorna el tamaño actual (elementos almacenados)"""
        return self.tamano_actual
    
    def obtener_capacidad(self):
        """Retorna la capacidad total del array"""
        return self.capacidad
    
    def listar_elementos(self):
        """Retorna lista con todos los elementos"""
        return [self.array[i] for i in range(self.tamano_actual)]
    
    def limpiar(self):
        """Vacía el array"""
        self.array = [None] * self.capacidad
        self.tamano_actual = 0
    
    def __len__(self):
        """Permite usar len() con el array"""
        return self.tamano_actual
    
    def __getitem__(self, indice):
        """Permite acceso con array[indice]"""
        return self.obtener(indice)
    
    def __setitem__(self, indice, valor):
        """Permite asignación con array[indice] = valor"""
        self.actualizar(indice, valor)
    
    def __str__(self):
        """Representación en string del array"""
        elementos = self.listar_elementos()
        return f"ArrayDinamico(tamano={self.tamano_actual}, capacidad={self.capacidad}, elementos={elementos})"