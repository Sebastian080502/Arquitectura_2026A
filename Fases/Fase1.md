# 🏗 Taller de Arquitectura de Software  
## Refactorización de Monolito – Encuestas Espagueti  

---

- Juan Sebastian Osorio Fierro
- Daniel Steve Fontalvo Matiz
- Leonado Fabio Perez Bermudez
- Juan Camilo Cruz Pardo

# 📌 FASE 1 – Montaje, Exploración y Pruebas Funcionales

## 1️⃣ Montaje del Entorno

# 1.1 Clonado del repositorio

Repositorio utilizado:
- https://github.com/lanvargas94/Semana-2


---

# 1.2 Construcción y ejecución del entorno

Comando ejecutado:

```bash
docker compose up --build -d

```

### CONSTRUCCIÓN DOCKER COMPOSE
![Construcción docker compose](./img/1.jpeg)

# 1.3 Verificación de contenedores activos

## Servicios levantados:

### 🐘 PostgreSQL → Puerto 5432

### ☕ Backend Spring Boot → Puerto 8081

### 🅰 Frontend Angular → Puerto 4200

# CONTENEDORES EN EJECUCIÓN
![Docker en ejecución](./img/2.jpeg)
![Docker en ejecución](./img/3.jpeg)


# 1.4 acceso a la aplicación 

Frontend: 
```bash
http://localhost:4200
```
![Frontend en ejecución](./img/4.jpeg)

Backend:
```bash
http://localhost:8081
```
![Backend en ejecución](./img/5.jpeg)


# 2 Pruebas funcionales

### CP-01 CREAR ENCUESTA VALIDA
![CP-01](./img/6.jpeg)

### CP-02 CREAR ENCUESTA CON TEXTO CARACTERES
![CP-02](./img/7.jpeg)

### CP-03 VOTAR "SI" EN UNA ENCUESTA
![CP-03](./img/8.jpeg)

### CP-04 VOTAR "NO" EN UNA ENCUESTA
![CP-04](./img/9.jpeg)

### CP-05 LISTAR TODAS LAS ENCUESTAS
![CP-05](./img/10.jpeg)

### CP-06 Probar SQL Injection (`' OR '1'='1`) 
![CP-06](./img/11.jpeg)
![CP-06](./img/12.jpeg)
![CP-06](./img/13.jpeg)

Se intentó explotar SQL Injection mediante path variable, pero el tipado fuerte de Spring (Integer id) impide que valores no numéricos lleguen a la consulta.
Sin embargo, el backend sigue siendo vulnerable debido a concatenación directa de SQL en los endpoints /crear y /votar.

EL endpoint es un INSERT, no un SELECT.
En un SELECT, ' OR '1'='1 altera la condición WHERE.
Pero aquí estás dentro de un VALUES (...), entonces PostgreSQL lo interpreta como string mal formado o como texto literal dependiendo del parsing.


| Caso  | Descripción                             | Resultado Esperado                       | Resultado Obtenido |
| ----- | --------------------------------------- | ---------------------------------------- | ------------------ |
| CP-01 | Crear encuesta válida                   | Encuesta creada                          | ✔ Correcto         |
| CP-02 | Crear encuesta con texto < 3 caracteres | Mensaje de error mostrado                | ❌Incorrecto       |
| CP-03 | Votar "SI" en una encuesta              | Contador de SI incrementa                | ✔ Correcto         |
| CP-04 | Votar "NO" en una encuesta              | Contador de NO incrementa                | ✔ Correcto         |
| CP-05 | Listar todas las encuestas              | Lista completa mostrada                  | ✔ Correcto         |
| CP-06 | Probar SQL Injection (`' OR '1'='1`)    | Sistema vulnerable                       | ⚠ Vulnerable       |

