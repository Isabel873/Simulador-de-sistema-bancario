# lista.py - Estructura Lista Simple

class Lista:
    def __init__(self):
        self.elementos = []
    
    def agregar(self, elemento):
        self.elementos.append(elemento)
    
    def buscar(self, criterio):
        for elemento in self.elementos:
            if criterio(elemento):
                return elemento
        return None
    
    def eliminar(self, elemento):
        if elemento in self.elementos:
            self.elementos.remove(elemento)
            return True
        return False
    
    def listar_todos(self):
        return self.elementos.copy()
    
    def tamano(self):
        return len(self.elementos)
    
    def esta_vacia(self):
        return len(self.elementos) == 0