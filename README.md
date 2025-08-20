# 📖 Guía de Greenbone OpenVAS (GVM)

## 📝 Introducción
**OpenVAS (Open Vulnerability Assessment Scanner)** es el motor de escaneo de vulnerabilidades de **Greenbone Vulnerability Management (GVM)**.  
Permite detectar vulnerabilidades en sistemas, servicios y dispositivos de red, proporcionando reportes detallados con CVEs, nivel de severidad y recomendaciones.

🔑 **Diferencia con Nmap**:  
- Nmap → Descubre hosts, puertos y servicios.  
- OpenVAS → Analiza vulnerabilidades conocidas, configuraciones inseguras y parches faltantes.  

---

## ⚙️ Instalación

### En Kali Linux
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install gvm -y
sudo gvm-setup
sudo gvm-check-setup
```

Acceso web:  
👉 `https://127.0.0.1:9392`

### En Ubuntu/Debian
```bash
sudo apt update
sudo apt install openvas -y
sudo gvm-setup
```

### Usando Docker
```bash
docker run -d -p 443:443 --name openvas mikesplain/openvas
```
Acceso en navegador: `https://127.0.0.1`  
Usuario: `admin` | Contraseña: `admin`

---

## 🔧 Configuración inicial

### Actualizar feeds
```bash
sudo gvm-feed-update
```

### Acceso web (GSA)
- Navegador → `https://IP-SERVIDOR:9392`  
- Usuario: `admin` (cambia la contraseña al primer inicio).  

### Crear un objetivo (Target)
1. `Configuration → Targets → New Target`  
2. Definir:  
   - Nombre: `Servidor-Web`  
   - Dirección IP: `192.168.1.100` (o rango `192.168.1.0/24`)  
   - Port List: `All IANA assigned TCP`  

### Crear tarea (Task)
1. `Scans → Tasks → New Task`  
2. Seleccionar:  
   - Target creado  
   - Scan Config: `Full and fast`  
3. Guardar y hacer clic en **Start**

---

## 📊 Analizar resultados

Cuando finalice un escaneo:  
- Ir a **Scans → Reports**  
- Revisar:  
  - **Summary** → Gráfico de severidad  
  - **Results** → Vulnerabilidades  
  - **CVEs** → Referencias de fallos  
  - **Ports** → Servicios detectados  
  - **Compliance** → Auditorías de configuración  

---

## 🌐 Conexión y monitoreo de dispositivos

### Escaneo de toda la red interna
Target con rango:
```
192.168.1.0/24
```

### Escaneo de dispositivos críticos
- Servidor Linux → `192.168.1.10`  
- Servidor Windows → `192.168.1.20`  
- Router → `192.168.1.1`  
- IoT → `192.168.1.50-60`  

### Escaneo autenticado (más profundo)
1. `Configuration → Credentials → New Credential`  
   - Tipo: **SSH** (Linux) o **SMB/WinRM** (Windows)  
   - Usuario/contraseña válidos  
2. Asociar credenciales al Target  
3. Resultado → OpenVAS detecta parches faltantes, configuraciones inseguras, etc.  

### Monitoreo periódico
- En `Scans → Tasks → Schedule`  
- Programar escaneos (ej: semanal)  

---

## 📤 Exportar y compartir reportes

Formatos disponibles:  
- **PDF** → Informe ejecutivo  
- **XML / CSV** → Análisis avanzado  
- **TXT** → Resumen plano  

Exportación desde **Scans → Reports → Export**  

Ejemplo CLI:
```bash
gvm-cli --gmp-username admin --gmp-password tu_pass socket --xml "<get_reports format_id='c1645568-627a-11e3-a660-406186ea4fc5'/>"
```

---

## 🛡️ Buenas prácticas

- Actualizar feeds:
  ```bash
  sudo gvm-feed-update
  ```
- Usar credenciales para más precisión.  
- Programar escaneos periódicos.  
- No escanear rangos enormes en producción.  
- Integrar con SIEM (Splunk, ELK).  
- Restringir acceso al panel web (VPN/firewall).  

---

## 🚀 Ejemplo de flujo completo

1. Descubrir dispositivos con Nmap:
   ```bash
   nmap -sn 192.168.1.0/24
   ```
2. Crear Target en OpenVAS con esas IPs.  
3. Configurar credenciales SSH/SMB.  
4. Ejecutar escaneo `Full and fast`.  
5. Analizar reporte y priorizar vulnerabilidades críticas.  
6. Programar escaneo semanal.  

---

## 📌 Comparativa rápida

| Herramienta | Objetivo principal         | Ejemplo de salida                          |
|-------------|---------------------------|---------------------------------------------|
| Nmap        | Descubrir hosts/puertos   | Puerto 22 abierto en 192.168.1.10          |
| OpenVAS     | Detectar vulnerabilidades | CVE-2023-12345 crítico en OpenSSH 7.2p2    |

---

## 📚 Recursos útiles

- [Documentación oficial Greenbone](https://greenbone.github.io/docs/)  
- [Repositorio OpenVAS](https://github.com/greenbone)  
- [Kali Linux GVM](https://www.kali.org/tools/gvm/)  
