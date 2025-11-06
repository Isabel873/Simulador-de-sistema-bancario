"""
ESTRUCTURA DE DATOS: ÁRBOL BINARIO DE BÚSQUEDA (BST)
Aplicación: Búsqueda eficiente de cuentas bancarias por número
"""

class NodoArbol:
    """Nodo individual del árbol binario"""
    
    def __init__(self, clave, valor):
        self.clave = clave      # Número de cuenta
        self.valor = valor      # Objeto Cuenta
        self.izquierda = None
        self.derecha = None


class ArbolBinarioBusqueda:
    """
    Árbol Binario de Búsqueda para almacenar cuentas.
    Propiedad: nodos izquierda < padre < nodos derecha
    Búsqueda promedio: O(log n)
    """
    
    def __init__(self):
        """Inicializa un árbol vacío"""
        self.raiz = None
        self.tamano_arbol = 0
    
    def insertar(self, clave, valor):
        """
        Inserta una nueva clave-valor en el árbol
        Complejidad: O(log n) promedio, O(n) peor caso
        """
        if self.raiz is None:
            self.raiz = NodoArbol(clave, valor)
        else:
            self._insertar_recursivo(self.raiz, clave, valor)
        self.tamano_arbol += 1
        return True
    
    def _insertar_recursivo(self, nodo, clave, valor):
        """Función auxiliar recursiva para insertar"""
        if clave < nodo.clave:
            if nodo.izquierda is None:
                nodo.izquierda = NodoArbol(clave, valor)
            else:
                self._insertar_recursivo(nodo.izquierda, clave, valor)
        elif clave > nodo.clave:
            if nodo.derecha is None:
                nodo.derecha = NodoArbol(clave, valor)
            else:
                self._insertar_recursivo(nodo.derecha, clave, valor)
        else:
            # La clave ya existe, actualizar valor
            nodo.valor = valor
            self.tamano_arbol -= 1  # No incrementamos tamaño
    
    def buscar(self, clave):
        """
        Busca una clave en el árbol
        Complejidad: O(log n) promedio
        Retorna: valor asociado o None
        """
        return self._buscar_recursivo(self.raiz, clave)
    
    def _buscar_recursivo(self, nodo, clave):
        """Función auxiliar recursiva para buscar"""
        if nodo is None:
            return None
        
        if clave == nodo.clave:
            return nodo.valor
        elif clave < nodo.clave:
            return self._buscar_recursivo(nodo.izquierda, clave)
        else:
            return self._buscar_recursivo(nodo.derecha, clave)
    
    def eliminar(self, clave):
        """
        Elimina un nodo del árbol
        Complejidad: O(log n) promedio
        """
        self.raiz, eliminado = self._eliminar_recursivo(self.raiz, clave)
        if eliminado:
            self.tamano_arbol -= 1
        return eliminado
    
    def _eliminar_recursivo(self, nodo, clave):
        """Función auxiliar recursiva para eliminar"""
        if nodo is None:
            return nodo, False
        
        if clave < nodo.clave:
            nodo.izquierda, eliminado = self._eliminar_recursivo(nodo.izquierda, clave)
            return nodo, eliminado
        elif clave > nodo.clave:
            nodo.derecha, eliminado = self._eliminar_recursivo(nodo.derecha, clave)
            return nodo, eliminado
        else:
            # Nodo encontrado, tres casos:
            
            # Caso 1: Nodo hoja
            if nodo.izquierda is None and nodo.derecha is None:
                return None, True
            
            # Caso 2: Un hijo
            if nodo.izquierda is None:
                return nodo.derecha, True
            if nodo.derecha is None:
                return nodo.izquierda, True
            
            # Caso 3: Dos hijos
            # Encontrar el sucesor (mínimo del subárbol derecho)
            sucesor = self._encontrar_minimo(nodo.derecha)
            nodo.clave = sucesor.clave
            nodo.valor = sucesor.valor
            nodo.derecha, _ = self._eliminar_recursivo(nodo.derecha, sucesor.clave)
            return nodo, True
    
    def _encontrar_minimo(self, nodo):
        """Encuentra el nodo con la clave mínima"""
        while nodo.izquierda:
            nodo = nodo.izquierda
        return nodo
    
    def recorrido_en_orden(self):
        """
        Recorrido In-Order (izquierda-raíz-derecha)
        Retorna elementos ordenados ascendentemente
        Complejidad: O(n)
        """
        resultado = []
        self._en_orden_recursivo(self.raiz, resultado)
        return resultado
    
    def _en_orden_recursivo(self, nodo, resultado):
        """Función auxiliar para recorrido en orden"""
        if nodo:
            self._en_orden_recursivo(nodo.izquierda, resultado)
            resultado.append((nodo.clave, nodo.valor))
            self._en_orden_recursivo(nodo.derecha, resultado)
    
    def recorrido_pre_orden(self):
        """
        Recorrido Pre-Order (raíz-izquierda-derecha)
        Complejidad: O(n)
        """
        resultado = []
        self._pre_orden_recursivo(self.raiz, resultado)
        return resultado
    
    def _pre_orden_recursivo(self, nodo, resultado):
        """Función auxiliar para recorrido pre-orden"""
        if nodo:
            resultado.append((nodo.clave, nodo.valor))
            self._pre_orden_recursivo(nodo.izquierda, resultado)
            self._pre_orden_recursivo(nodo.derecha, resultado)
    
    def recorrido_post_orden(self):
        """
        Recorrido Post-Order (izquierda-derecha-raíz)
        Complejidad: O(n)
        """
        resultado = []
        self._post_orden_recursivo(self.raiz, resultado)
        return resultado
    
    def _post_orden_recursivo(self, nodo, resultado):
        """Función auxiliar para recorrido post-orden"""
        if nodo:
            self._post_orden_recursivo(nodo.izquierda, resultado)
            self._post_orden_recursivo(nodo.derecha, resultado)
            resultado.append((nodo.clave, nodo.valor))
    
    def altura(self):
        """
        Calcula la altura del árbol
        Complejidad: O(n)
        """
        return self._calcular_altura(self.raiz)
    
    def _calcular_altura(self, nodo):
        """Función auxiliar recursiva para calcular altura"""
        if nodo is None:
            return 0
        return 1 + max(self._calcular_altura(nodo.izquierda), 
                       self._calcular_altura(nodo.derecha))
    
    def esta_vacio(self):
        """Verifica si el árbol está vacío"""
        return self.raiz is None
    
    def tamano(self):
        """Retorna el número de nodos en el árbol"""
        return self.tamano_arbol
    
    def limpiar(self):
        """Vacía completamente el árbol"""
        self.raiz = None
        self.tamano_arbol = 0
    
    def __len__(self):
        """Permite usar len() con el árbol"""
        return self.tamano_arbol
    
    def __str__(self):
        """Representación en string del árbol"""
        elementos = self.recorrido_en_orden()
        return f"ArbolBST(tamano={self.tamano_arbol}, elementos={elementos})"