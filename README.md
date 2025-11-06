"ESTRUCTURA DE DATOS: LISTA ENLAZADA"
"Aplicación: Registro y gestión de clientes del banco"

class NodoLista:
    """Nodo individual de la lista enlazada"""
    
    def __init__(self, dato):
        self.dato = dato
        self.siguiente = None


class ListaEnlazada:
    """
    Lista enlazada simple para almacenar clientes.
    Permite inserción, búsqueda y eliminación eficientes.
    """
    
    def __init__(self):
        """Inicializa una lista vacía"""
        self.cabeza = None
        self.tamano_lista = 0
    
    def esta_vacia(self):
        """Verifica si la lista está vacía"""
        return self.cabeza is None
    
    def agregar(self, dato):
        """
        Agrega un elemento al final de la lista
        Complejidad: O(n) - debe recorrer hasta el final
        """
        nuevo_nodo = NodoLista(dato)
        
        if self.esta_vacia():
            self.cabeza = nuevo_nodo
        else:
            actual = self.cabeza
            while actual.siguiente:
                actual = actual.siguiente
            actual.siguiente = nuevo_nodo
        
        self.tamano_lista += 1
        return True
    
    def agregar_al_inicio(self, dato):
        """
        Agrega un elemento al inicio de la lista
        Complejidad: O(1)
        """
        nuevo_nodo = NodoLista(dato)
        nuevo_nodo.siguiente = self.cabeza
        self.cabeza = nuevo_nodo
        self.tamano_lista += 1
        return True
    
    def buscar(self, criterio):
        """
        Busca un elemento que cumpla el criterio (función lambda)
        Complejidad: O(n)
        """
        actual = self.cabeza
        while actual:
            if criterio(actual.dato):
                return actual.dato
            actual = actual.siguiente
        return None
    
    def eliminar(self, criterio):
        """
        Elimina el primer elemento que cumpla el criterio
        Complejidad: O(n)
        """
        if self.esta_vacia():
            return False
        
        # Si es el primer elemento
        if criterio(self.cabeza.dato):
            self.cabeza = self.cabeza.siguiente
            self.tamano_lista -= 1
            return True
        
        # Buscar en el resto de la lista
        actual = self.cabeza
        while actual.siguiente:
            if criterio(actual.siguiente.dato):
                actual.siguiente = actual.siguiente.siguiente
                self.tamano_lista -= 1
                return True
            actual = actual.siguiente
        
        return False
    
    def listar_todos(self):
        """
        Retorna una lista con todos los elementos
        Complejidad: O(n)
        """
        elementos = []
        actual = self.cabeza
        while actual:
            elementos.append(actual.dato)
            actual = actual.siguiente
        return elementos
    
    def tamano(self):
        """Retorna el tamaño de la lista"""
        return self.tamano_lista
    
    def limpiar(self):
        """Vacía completamente la lista"""
        self.cabeza = None
        self.tamano_lista = 0
    
    def __len__(self):
        """Permite usar len() con la lista"""
        return self.tamano_lista
    
    def __str__(self):
        """Representación en string de la lista"""
        elementos = self.listar_todos()
        return f"ListaEnlazada({elementos})"