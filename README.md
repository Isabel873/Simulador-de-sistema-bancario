"""
PRUEBAS UNITARIAS
Valida el correcto funcionamiento de todas las estructuras y clases.

Comandos para ejecutar:
    pytest tests/test_sistema_bancario.py -v
    pytest tests/ -v --cov=src
"""

import pytest
import sys
import os

# Agregar el directorio src al path para las importaciones
sys.path.append(os.path.join(os.path.dirname(__file__), '..', 'src'))

# Importaciones REALES de tus clases
from estructuras.pila import Pila
from estructuras.cola import Cola
from estructuras.lista import ListaEnlazada
from estructuras.arbol_binario import ArbolBinarioBusqueda
from modelos.cliente import Cliente
from modelos.cuenta import CuentaAhorro, CuentaCorriente
from modelos.transaccion import Transaccion
from sistema_bancario import SistemaBancario


# ========== TESTS PARA COLA (FIFO) ==========
class TestCola:
    """Tests para estructura Cola FIFO"""

    def test_crear_cola_vacia(self):
        cola = Cola()
        assert cola.esta_vacia()
        assert len(cola) == 0

    def test_encolar_elementos(self):
        cola = Cola()
        cola.encolar(1)
        cola.encolar(2)
        cola.encolar(3)
        assert len(cola) == 3
        assert not cola.esta_vacia()

    def test_desencolar_fifo(self):
        cola = Cola()
        cola.encolar("primero")
        cola.encolar("segundo")
        cola.encolar("tercero")

        primero = cola.desencolar()
        segundo = cola.desencolar()

        assert primero == "primero"
        assert segundo == "segundo"
        assert len(cola) == 1

    def test_ver_primero_sin_remover(self):
        cola = Cola()
        cola.encolar(10)
        cola.encolar(20)
        cola.encolar(30)

        primero = cola.frente()
        assert primero == 10
        assert len(cola) == 3

    def test_cola_vacia_desencolar(self):
        cola = Cola()
        with pytest.raises(IndexError):
            cola.desencolar()


# ========== TESTS PARA PILA (LIFO) ==========
class TestPila:
    """Tests para estructura Pila LIFO"""

    def test_crear_pila_vacia(self):
        pila = Pila()
        assert pila.esta_vacia()
        assert len(pila) == 0

    def test_apilar_elementos(self):
        pila = Pila()
        pila.apilar(1)
        pila.apilar(2)
        pila.apilar(3)
        assert len(pila) == 3
        assert not pila.esta_vacia()

    def test_desapilar_lifo(self):
        pila = Pila()
        pila.apilar("primero")
        pila.apilar("segundo")
        pila.apilar("tercero")

        ultimo = pila.desapilar()
        penultimo = pila.desapilar()

        assert ultimo == "tercero"
        assert penultimo == "segundo"
        assert len(pila) == 1

    def test_ver_tope_sin_remover(self):
        pila = Pila()
        pila.apilar(10)
        pila.apilar(20)
        pila.apilar(30)

        tope = pila.cima()
        assert tope == 30
        assert len(pila) == 3

    def test_pila_vacia_desapilar(self):
        pila = Pila()
        with pytest.raises(IndexError):
            pila.desapilar()


# ========== TESTS PARA LISTA ENLAZADA ==========
class TestListaEnlazada:
    """Tests para Lista Enlazada"""

    def test_crear_lista_vacia(self):
        lista = ListaEnlazada()
        assert len(lista) == 0

    def test_agregar_elementos(self):
        lista = ListaEnlazada()
        lista.agregar("elemento1")
        lista.agregar("elemento2")
        lista.agregar("elemento3")
        assert len(lista) == 3

    def test_buscar_elemento(self):
        lista = ListaEnlazada()
        lista.agregar("cliente1")
        lista.agregar("cliente2")
        lista.agregar("cliente3")

        encontrado = lista.buscar(lambda x: x == "cliente2")
        no_encontrado = lista.buscar(lambda x: x == "inexistente")

        assert encontrado == "cliente2"
        assert no_encontrado is None

    def test_recorrer_elementos(self):
        lista = ListaEnlazada()
        lista.agregar(1)
        lista.agregar(2)
        lista.agregar(3)
        elementos = lista.recorrer()
        assert elementos == [1, 2, 3]


# ========== TESTS PARA ÁRBOL BINARIO ==========
class TestArbolBinario:
    """Tests para Árbol Binario de Búsqueda"""

    def test_crear_arbol_vacio(self):
        arbol = ArbolBinarioBusqueda()
        assert arbol.raiz is None

    def test_insertar_elementos(self):
        arbol = ArbolBinarioBusqueda()
        arbol.insertar(50, "valor50")
        arbol.insertar(30, "valor30")
        arbol.insertar(70, "valor70")

        assert arbol.buscar(50) == "valor50"
        assert arbol.buscar(30) == "valor30"
        assert arbol.buscar(70) == "valor70"

    def test_buscar_elemento(self):
        arbol = ArbolBinarioBusqueda()
        arbol.insertar(50, "valor50")
        arbol.insertar(30, "valor30")
        arbol.insertar(70, "valor70")
        assert arbol.buscar(30) == "valor30"

    def test_buscar_elemento_inexistente(self):
        arbol = ArbolBinarioBusqueda()
        arbol.insertar(50, "valor50")
        assert arbol.buscar(99) is None

    def test_recorrido_inorden(self):
        arbol = ArbolBinarioBusqueda()
        arbol.insertar(50, "50")
        arbol.insertar(30, "30")
        arbol.insertar(70, "70")
        arbol.insertar(20, "20")
        arbol.insertar(40, "40")
        assert arbol.inorden() == ["20", "30", "40", "50", "70"]


# ========== TESTS PARA CLIENTE ==========
class TestCliente:
    """Tests para clase Cliente"""

    def test_crear_cliente(self):
        cliente = Cliente("1", "Juan Pérez", "1234567890", [], "2024-01-01", "juan@email.com", "0999999999")
        assert cliente.id == "1"
        assert cliente.nombre == "Juan Pérez"
        assert cliente.email == "juan@email.com"
        assert isinstance(cliente.cuentas, list)

    def test_agregar_cuenta(self):
        cliente = Cliente("1", "Test", "0000000000", [], "2024-01-01", "test@email.com", "0999999999")
        cuenta_mock = object()
        cliente.agregar_cuenta(cuenta_mock)
        assert cuenta_mock in cliente.cuentas

    def test_get_saldo_total(self):
        cliente = Cliente("1", "Test", "0000000000", [], "2024-01-01", "test@email.com", "0999999999")
        cuenta1 = type("MockCuenta", (), {"saldo": 1000})()
        cuenta2 = type("MockCuenta", (), {"saldo": 500})()
        cliente.cuentas.extend([cuenta1, cuenta2])
        assert cliente.get_saldo_total() == 1500


# ========== TESTS PARA SISTEMA BANCARIO ==========
class TestSistemaBancario:
    """Tests de integración del Sistema Bancario"""

    def test_crear_sistema_bancario(self):
        sistema = SistemaBancario()
        assert isinstance(sistema, SistemaBancario)
        assert sistema.clientes == []

    def test_agregar_cliente(self):
        sistema = SistemaBancario()
        cliente = sistema.agregar_cliente("Ana", "123", "2024-01-01", "ana@email.com", "0999999999")
        assert cliente.nombre == "Ana"
        assert len(sistema.clientes) == 1

    def test_crear_cuenta_y_buscar(self):
        sistema = SistemaBancario()
        cliente = sistema.agregar_cliente("Carlos", "456", "2024-01-01", "carlos@email.com", "0998888888")
        cuenta = sistema.crear_cuenta_ahorro(cliente, 1000.0)
        assert sistema.buscar_cuenta(cuenta.numero) == cuenta


# ========== MAIN ==========
if __name__ == "__main__":
    pytest.main(["-v"])
