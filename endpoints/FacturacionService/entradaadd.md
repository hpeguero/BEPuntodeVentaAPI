# API Documentation - EntradaAdd Controller

## Índice
- [Endpoint](#endpoint)
- [Descripción](#descripción)
- [Parámetros de Entrada](#parámetros-de-entrada)
- [Funcionalidades Principales](#funcionalidades-principales)
- [Respuesta](#respuesta)
- [Códigos de Estado HTTP](#códigos-de-estado-http)
- [Integraciones Externas](#integraciones-externas)
- [Servicios Utilizados](#servicios-utilizados)
- [Referencias](#referencias)

## Endpoint
```http
POST /EntradaAdd
```

## Descripción
Pendiente descripcion - crea registro de entrada...

## Parámetros de Entrada

### Body Parameters
| Parámetro | Tipo | Descripción | Requerido |
|-----------|------|-------------|-----------|
| `entradas` | `string` | JSON serializado que contiene toda la información para crear la entrada | ✅ |

### Estructura del JSON de Entrada (`entradas`)
```json
{
  "id": "string (GUID)",
  "usuarioLog": {
    "nombreUsuario": "string",
    "nombreSucursal": "string"
  },
  "entradas": [
    {
      "maestra": {
        "entradaid": "string",
        "nombreCompleto": "string (URL encoded)",
        "nombre": "string (URL encoded)",
        "apellido": "string (URL encoded)",
        "direccion": "string (URL encoded)",
        "doctorId": "number (opcional)",
        "doctorNombre": "string (URL encoded, opcional)",
        "nombreDiagnostico": "string (URL encoded, opcional)"
      }
    }
  ],
  "citas": "object (opcional)"
}
```

> **📖 Referencias:** [Esquemas JSON](./schemas/entrada-request.json) | [Validación de Datos](./validation-rules.md)

## Funcionalidades Principales

### 1. Procesamiento de Entradas
- Decodifica parámetros URL encoded
- Convierte JSON a XML para procesamiento en base de datos  
- Ejecuta stored procedure `Facturacion.UspEntradaAdd`

> **📋 Ver:** [Documentación de Stored Procedures](./database/stored-procedures.md#uspentradaadd)

### 2. Link de Resultados
- Crea enlaces entre facturas y resultados de laboratorio
- Procesa códigos de seguimiento
- Maneja información de NCF (Número de Comprobante Fiscal)

> **🔗 Relacionado:** [Servicio de Resultados](./services/resultados-service.md) | [Gestión de Enlaces](./linking-system.md)

### 3. Integración con NCF
- Obtiene códigos de seguridad y firmas
- Gestiona e-NCF y NCF tradicionales  
- Actualiza información fiscal en facturas

> **💼 Documentación:** [API de NCF](./integrations/ncf-service.md) | [Comprobantes Fiscales](./fiscal/ncf-guide.md)

### 4. Integración con PowerI (Sistema de Laboratorio)
- Envía información de laboratorios para procesamiento
- Obtiene fechas promesa de pruebas
- Maneja timeouts y errores de conexión

> **⚡ PowerI:** [API de PowerI](./integrations/poweri-api.md) | [Configuración de Laboratorio](./lab-system/poweri-setup.md)

### 5. Gestión de Doctores (SalesForce)
- Valida y crea doctores temporales
- Envía notificaciones por email
- Integra con sistema SalesForce para doctores preliminares

> **👨‍⚕️ SalesForce:** [Integración SalesForce](./integrations/salesforce-integration.md) | [Gestión de Doctores](./doctor-management.md)

### 6. Procesamiento de Citas
- Registra citas médicas asociadas
- Ejecuta stored procedure `UspCitaAddUpd`

> **📅 Citas:** [Sistema de Citas](./appointments/appointment-system.md) | [API de Citas](./api/appointments-api.md)

## Respuesta

### Respuesta Exitosa
**Tipo:** `object` (JSON serializado)
**Código HTTP:** `200 OK`

```json
{
  "Facturas": [
    {
      "GlobalID": "string",
      "NumeroFactura": "string",
      "NumeroLaboratorio": "string",
      "LinkResultadosID": "string",
      "PowerIProc": "boolean",
      "NCFTipoFinal": "string",
      "JsonNCF": "string",
      "UsuarioID": "string",
      "CodSucursal": "number",
      "LinkResultados": "boolean",
      "FirmaCodigoSeg": "string",
      "NCFTipo": "string",
      "NCF": "string",
      "FirmaCodigoQR": "string",
      "FirmaFecha": "datetime",
      "DoctorTmpID": "number",
      "CodigoFactura": "string",
      "Pruebas": [
        {
          "Prueba_CodigoPower": "string",
          "Prueba_FechaPromesa": "datetime"
        }
      ]
    }
  ],
  "PowerIJson": "array (opcional)"
}
```

> **📊 Ver también:** [Esquemas de Respuesta](./schemas/entrada-response.json) | [Códigos de Error](./error-handling.md)

### Respuesta de Error
**Tipo:** `null`
**Código HTTP:** `500 Internal Server Error`

En caso de error, se registra la excepción y se retorna `null`.

> **🚨 Manejo de Errores:** [Guía de Manejo de Errores](./error-handling.md) | [Logs de Sistema](./logging/system-logs.md)

## Códigos de Estado HTTP

| Código | Descripción |
|--------|-------------|
| `200` | Entrada procesada exitosamente |
| `500` | Error interno del servidor |

## Integraciones Externas

### APIs Utilizadas
1. **PowerI API** (`corePowerApi`)
   - Endpoints: `/Interfaz`, `/GetFechaPromesaList`
   - Timeout: 30 segundos
   
2. **Doctor API** (`coreDoctorApi`)
   - Endpoints: `/api/SalesForces/Token`, `/api/SalesForces/CrearDoctorPreliminarPVWEB`

### Stored Procedures
- `Facturacion.UspEntradaAdd`
- `UspEntradaProc`
- `UspEntradaFechaPromesa`
- `UspCitaAddUpd`
- `UspHtmlTemplatesGet`
- `UspDoctorTempValidate`

## Servicios Utilizados
- `UsuarioService` - Gestión de usuarios y logging
- `ResultadosService` - Procesamiento de resultados
- `NCFService` - Gestión de comprobantes fiscales
- `EmailService` - Envío de notificaciones
- `ExceptionService` - Manejo de excepciones

---

## Referencias

### Documentación Relacionada
- [🏠 **Inicio**](./README.md) - Documentación principal del proyecto

---

> **📞 Soporte:** Si necesitas ayuda adicional, consulta la [Guía de Soporte](./support.md) o contacta al equipo de desarrollo.
