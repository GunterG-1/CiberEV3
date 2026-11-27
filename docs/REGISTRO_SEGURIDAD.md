# Registro de seguridad · Sistema Alumnos

Estudiante: Gunter Garete___   Repositorio: (https://github.com/GunterG-1/CiberEV3)   Fecha: 27-11-2026

Completa este registro a medida que avanzas. Cada fila debe poder seguirse hasta un **build de Jenkins**
y un **commit** (usa el identificador `VULN-xx` en el mensaje del commit).

## 1. Revisiones de seguridad realizadas

| # | Etapa del SDLC | Herramienta / técnica | Build | Resultado (resumen) |
|---|---|---|---|---|
| R1 | Código (SAST) | Bandit local y Jenkins | Inicial local / #2; verificación final #7 | Inicialmente se detectaron hallazgos en el código; en #7 Bandit quedó sin hallazgos Medium/High |
| R2 | Dependencias (SCA) | pip-audit local y Jenkins | Inicial local / #2; verificación final #8 | Inicialmente 19 vulnerabilidades en Flask, Werkzeug, Jinja2 y gunicorn; en #8 no se encontraron vulnerabilidades |
| R3 | Pruebas dinámicas (DAST) | OWASP ZAP sobre staging | Pendiente | No ejecutado todavía; el Jenkinsfile aún no incluye las etapas Deploy Staging y DAST |
| R4 | Revisión manual | Revisión del código de `app.py` | — | Se identificó una clave codificada; corregida en VULN-01, commit `0fa9add`, build #3 |
| R5 | Producción | Grafana + Prometheus | Pendiente | Pendiente de recopilar evidencia de monitoreo |

## 2. Vulnerabilidades identificadas y corregidas

| ID | Vulnerabilidad (CWE) | Detectada por | Ubicación | Riesgo | Corrección aplicada | Commit | Build que lo verifica |
|---|---|---|---|---|---|---|---|
| VULN-01 | Clave secreta codificada (CWE-798) | Revisión manual del código | `app.py` (clave retirada) | High | Se eliminó la clave codificada del repositorio | `0fa9add` | #3 |
| VULN-02 | Inyección SQL (CWE-89) | Bandit B608, revisión local | `app.py:203` | Medium | Consulta parametrizada mediante `?` y parámetro enlazado | `071b833` | #4 |
| VULN-03 | Uso de hash débil MD5 (CWE-327) | Bandit B324, revisión local | `app.py:219-220` | High | Reemplazado por `check_password_hash` con hash desde variable de entorno | `760af5a` | #5 |
| VULN-04 | Flask en modo debug (CWE-94) | Bandit B201, revisión local | `app.py:241` | High | Se desactivó con `debug=False` | `1245630` | #6 |
| VULN-05 | Escucha en todas las interfaces (CWE-605) | Bandit B104, revisión local | `app.py:241` | Medium | Se limitó el host local a `127.0.0.1` | `a13247d` | #7 |
| VULN-06 | Dependencias con vulnerabilidades conocidas (19 avisos PYSEC; ver §5) | pip-audit, revisión local y Jenkins | `requirements.txt` | Variable según aviso | Se actualizaron Flask, Werkzeug, Jinja2 y gunicorn a versiones corregidas | `f373580` | #8 |

(Agrega las filas que necesites.)

## 3. Resultados de OWASP ZAP

| Alerta de ZAP | Riesgo | URL de ejemplo | VULN relacionada | Cómo se mitiga | ¿Mitigada? Build |
|---|---|---|---|---|---|
| | | | | | |

## 4. Monitoreo e incidentes

| Hora | Panel / métrica de Grafana | Qué se observó | Entorno | Clasificación | Acción de respuesta | Verificación |
|---|---|---|---|---|---|---|
| | | | | | | |

## 5. Gestión de dependencias

| Paquete | Versión inicial | Vulnerabilidades (IDs) | Versión final | Origen del cambio (PR de Dependabot / manual) | ¿Pruebas, pip-audit y ZAP siguen OK? Build |
|---|---|---|---|---|---|
| Flask | 2.2.2 | PYSEC-2023-62; PYSEC-2026-2151 | 3.1.3 | Manual (`f373580`) | Pruebas OK, pip-audit sin vulnerabilidades; ZAP pendiente, build #8 |
| Werkzeug | 2.2.2 | PYSEC-2023-58; PYSEC-2023-57; PYSEC-2023-221; PYSEC-2026-2046; PYSEC-2026-2045; PYSEC-2026-2044; PYSEC-2026-2043; PYSEC-2026-2320; PYSEC-2026-3417; PYSEC-2026-4312 | 3.1.9 | Manual (`f373580`) | Pruebas OK, pip-audit sin vulnerabilidades; ZAP pendiente, build #8 |
| Jinja2 | 3.1.2 | PYSEC-2026-1473; PYSEC-2026-1471; PYSEC-2026-1474; PYSEC-2026-1475; PYSEC-2026-1472 | 3.1.6 | Manual (`f373580`) | Pruebas OK, pip-audit sin vulnerabilidades; ZAP pendiente, build #8 |
| gunicorn | 21.2.0 | PYSEC-2026-1434; PYSEC-2026-1433 | 22.0.0 | Manual (`f373580`) | Pruebas OK, pip-audit sin vulnerabilidades; ZAP pendiente, build #8 |
| prometheus-client | 0.17.1 | Sin vulnerabilidades reportadas por pip-audit | 0.17.1 | Sin cambios | pip-audit sin vulnerabilidades; ZAP pendiente, build #8 |

## 6. Trazabilidad

Pega aquí (o adjunta) el `TRAZABILIDAD.md` del último build y completa un recorrido por cada actividad del pipeline:

| Actividad | Hallazgo | Build que lo detectó | Commit / PR que lo resolvió | Build que lo verificó | Versión en producción |
|---|---|---|---|---|---|
| Revisión de seguridad (Bandit) | | #__ | | #__ | |
| Prueba automatizada (OWASP ZAP) | | #__ | | #__ | |
| Gestión de dependencias (Dependabot) | | #__ | PR #__ | #__ | |
