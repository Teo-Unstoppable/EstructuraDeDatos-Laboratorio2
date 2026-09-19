# Laboratorio 2 — Árbol de Merkle (Merkle Tree)

Implementación de un Árbol de Merkle con SHA-256, incluyendo construcción,
demostración de sensibilidad a cambios y pruebas de inclusión (Merkle Proofs).

## Estructura del repositorio

```
.
├── merkle_tree.py       # Implementación del árbol (MerkleTree, ProofStep)
├── main.py              # Script del experimento solicitado
├── diagrama_arbol.txt    # Diagrama ASCII del árbol construido
└── README.md
```

## Especificación implementada

- Cada **hoja** = `SHA-256(bloque_de_datos)`.
- Cada **nodo interno** = `SHA-256(hash_izquierdo + hash_derecho)`.
- Si un nivel tiene número **impar** de nodos, el último se **duplica**
  antes de calcular el nivel superior.
- La **Merkle Root** es el hash de la raíz, representa la integridad de
  todo el conjunto de datos.



## Ejecución

```bash
python3 main.py
```

Esto ejecuta el experimento completo e imprime los resultados en consola
(el contenido de `output_log.txt` es una copia de esa salida).

## Experimento y resultados

1. **5 transacciones simuladas** (`TX1`…`TX5`), representando transferencias.
2. **Construcción del árbol y Merkle Root**:
   ```
   Merkle Root: 4bf067a40bf2137c98738e26730915f73b8352a5f5469cf74a07c45e77cc7dba
   ```
   Ver `diagrama_arbol.txt` para el árbol completo.
3. **Modificación de una transacción** (`TX4`, monto alterado): la raíz
   cambia por completo:
   ```
   Root original : 4bf067a40bf2137c98738e26730915f73b8352a5f5469cf74a07c45e77cc7dba
   Root modificada: 1392d651f0e183f7add9c061ab2a1ea61caecbee00a4807a93462df91d1a333f
   ```
   Esto demuestra el **efecto avalancha**: un cambio mínimo en un dato
   invalida toda la raíz, sin necesidad de recalcular el árbol completo
   para detectarlo.
4. **Prueba de inclusión (Merkle Proof) para la Transacción 3**: se genera
   la lista de hashes hermanos necesarios para reconstruir la raíz a
   partir de `TX3`, y se verifica con éxito:
   ```
   Resultado de verificación (dato correcto): True
   ```
5. **Verificación con un dato incorrecto**: usando la misma prueba pero
   cambiando el contenido de `TX3`, la verificación **falla**, como se
   espera:
   ```
   Resultado de verificación (dato incorrecto): False
   ```

## Capturas de pantalla


## Diseño del código

- `merkle_tree.py`
  - `sha256_hex(data)`: helper de hashing.
  - `MerkleTree`: construye el árbol nivel por nivel (`self.levels`),
    aplicando la regla de duplicación cuando un nivel es impar. Expone
    `root`, `height`, `get_proof(index)` y el método estático
    `verify_proof(data, proof, root)`.
  - `ProofStep`: cada paso de una prueba de inclusión guarda el hash
    hermano y el lado (`left`/`right`) en el que debe concatenarse.
  - `render_ascii()`: genera la representación ASCII del árbol.
- `main.py`: orquesta los 5 pasos del experimento solicitado en el enunciado.

Nota: Se uso Gemini Flash en Colab en una parte del código y para la redacción de la documentación el README.
