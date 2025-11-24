# Documentación de Implementación: Server TCP V4

Este documento detalla la implementación de los scripts de control para el servidor `serverTCPV4.py`.

## Archivos del Proyecto

### Scripts de Control
Se han creado tres scripts de Bash para gestionar el ciclo de vida del servidor, siguiendo el estándar del proyecto:

1.  **`server_start_tcpv4.sh`**: Inicia el servidor.
    *   Verifica dependencias (Python 3, módulos).
    *   Evita ejecuciones múltiples (verifica PID file).
    *   Ejecuta el servidor en segundo plano usando `nohup`.
    *   Los logs se guardan automáticamente en `logs/LOG_ddmmyy`.
    *   Guarda el PID en `/tmp/serverTCPV4.pid`.

2.  **`server_stop_tcpv4.sh`**: Detiene el servidor.
    *   Lee el PID desde `/tmp/serverTCPV4.pid`.
    *   Envía señal `TERM` para cierre ordenado.
    *   Si falla, fuerza el cierre con `KILL` tras un tiempo de espera.
    *   Limpia el archivo PID.

3.  **`server_status_tcpv4.sh`**: Muestra el estado.
    *   Verifica si el proceso está corriendo.
    *   Muestra estadísticas de uso (CPU, Memoria).
    *   Verifica si el puerto 5000 está escuchando.
    *   Muestra las últimas líneas del log del día actual.

### Sistema de Logs
*   **Formato**: `logs/LOG_ddmmyy` (ejemplo: `logs/LOG_241124` para 24 de noviembre de 2024)
*   **Ubicación**: Directorio `logs/` (se crea automáticamente)
*   **Rotación**: Automática por fecha (un archivo por día)

### Archivos de Datos
*   **`serverTCPV4.py`**: Script principal del servidor.
*   **`funciones.py`**: Módulo con funciones de logging y utilidades.
*   **`protocolo.py`**: Módulo de protocolo.
*   **`logs/LOG_ddmmyy`**: Archivos de log diarios.
*   **`/tmp/serverTCPV4.pid`**: Archivo temporal que almacena el ID del proceso.

## Guía de Uso

### 1. Iniciar el Servidor
```bash
./server_start_tcpv4.sh
```
Si el inicio es correcto, verá un mensaje de confirmación y el PID del proceso.

### 2. Verificar Estado
```bash
./server_status_tcpv4.sh
```
Este comando le mostrará si el servidor está "Ejecutándose" o "No ejecutándose", junto con estadísticas vitales.

### 3. Detener el Servidor
```bash
./server_stop_tcpv4.sh
```
Detiene el proceso de manera segura.

### 4. Ver Logs en Tiempo Real
Puede usar el comando `tail` para ver lo que está ocurriendo en el servidor:
```bash
# Log del día actual
tail -f logs/LOG_$(date +%d%m%y)

# O especificar una fecha
tail -f logs/LOG_241124
```

### 5. Listar Logs Históricos
```bash
ls -lh logs/
```

## Notas de Mantenimiento
*   Los scripts asumen que `python3` está instalado y disponible en el path.
*   El puerto configurado por defecto es el **5000** (verificado en `server_status_tcpv4.sh`).
*   Los logs se rotan automáticamente por fecha, un archivo nuevo se crea cada día.
*   El directorio `logs/` se crea automáticamente si no existe.
*   Si necesita cambiar la configuración, edite las variables al inicio de cada script `.sh`.
