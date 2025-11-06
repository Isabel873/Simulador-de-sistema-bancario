# Simulador-de-sistema-bancario
Desarrollar un sistema bancario completo 

"""
ESTRUCTURA DE DATOS: COLA (FIFO - First In First Out)
Aplicación: Procesar transacciones bancarias en orden de llegada
"""

class Cola:
    """
    Cola FIFO para gestionar transacciones pendientes.
    Primer elemento en entrar es el primero en salir.
    """
    
    def __init__(self):
        """Inicializa una cola vacía usando lista de Python"""
        self.items = []
    
    def encolar(self, item):
        """
        Agrega un elemento al final de la cola
        Complejidad: O(1) amortizado
        """
        self.items.append(item)
        return True
    
    def desencolar(self):
        """
        Remueve y retorna el primer elemento de la cola
        Complejidad: O(n) debido a reorganización de lista
        """
        if not self.esta_vacia():
            return self.items.pop(0)
        raise IndexError("No se puede desencolar: cola vacía")
    
    def ver_primero(self):
        """
        Retorna el primer elemento sin removerlo
        Complejidad: O(1)
        """
        if not self.esta_vacia():
            return self.items[0]
        return None
    
    def esta_vacia(self):
        """Verifica si la cola está vacía"""
        return len(self.items) == 0
    
    def tamano(self):
        """Retorna el número de elementos en la cola"""
        return len(self.items)
    
    def limpiar(self):
        """Vacía completamente la cola"""
        self.items = []
    
    def __str__(self):
        """Representación en string de la cola"""
        return f"Cola({self.items})"
    
    def __len__(self):
        """Permite usar len() con la cola"""
        return self.tamano()