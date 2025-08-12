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
