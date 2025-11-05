# Simulador-de-sistema-bancario
Desarrollar un sistema bancario completo 

class Pila:
    def __init__(self):
        self._elementos = []
    
    def apilar(self, elemento):
        self._elementos.append(elemento)
    
    def desapilar(self):
        if self.esta_vacia():
            raise IndexError("La pila está vacía")
        return self._elementos.pop()
    
    def cima(self):
        if self.esta_vacia():
            return None
        return self._elementos[-1]
    
    def esta_vacia(self):
        return len(self._elementos) == 0
    
    def __len__(self):
        return len(self._elementos)
    
    def __str__(self):
        return str(self._elementos)