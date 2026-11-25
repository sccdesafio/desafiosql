# Documentación de Pruebas Automatizadas (Playwright)

## 1. Descripción General
Suite de pruebas automatizadas End-to-End (E2E) desarrollada con **Playwright** para validar el flujo crítico de compra en la plataforma **SauceDemo** (`https://saucedemo.com`), verificando autenticación, selección de productos, carrito de compras y finalización de orden (*checkout*).

---

## 2. Requisitos e Instalación
* **Node.js** (v18 o superior)
* **Playwright**

Para instalar las dependencias del proyecto desde la raíz del repositorio:

```bash
npm install
npx playwright install
```

---

## 3. Estructura del Proyecto

```text
├── docs/
│   ├── entregable_sql.md        # Scripts DDL/DML y consultas SQL en Oracle
│   └── pruebas_playwright.md    # Documentación de automatización E2E
├── tests/
│   └── checkout.spec.ts         # Casos de prueba del flujo de compra
├── playwright.config.ts         # Configuración de navegadores, reportes y timeouts
└── package.json
```

---

## 4. Casos de Prueba Implementados

| ID | Caso de Prueba | Precondiciones | Pasos Ejecutados | Resultado Esperado |
| :--- | :--- | :--- | :--- | :--- |
| **TC-01** | Inicio de sesión exitoso | Usuario válido (`standard_user`) | 1. Ingresar a la URL.<br>2. Completar usuario y contraseña.<br>3. Hacer clic en **Login**. | Redirección al catálogo de productos (`/inventory.html`). |
| **TC-02** | Selección de productos y carrito | Sesión iniciada | 1. Agregar productos al carrito (*Sauce Labs Backpack*, *Bike Light*, *Onesie*, etc.).<br>2. Navegar al ícono del carrito. | Los productos agregados y sus precios coinciden con el catálogo. |
| **TC-03** | Proceso de Checkout completo | Productos en el carrito | 1. Clic en **Checkout**.<br>2. Completar Nombre, Apellido y Código Postal.<br>3. Validar resumen y clic en **Finish**. | Se muestra el mensaje de confirmación: *"Thank you for your order!"*. |

---

## 5. Comandos de Ejecución

* **Ejecutar todas las pruebas (modo headless):**
  ```bash
  npx playwright test
  ```

* **Ejecutar pruebas viendo el navegador (modo headed):**
  ```bash
  npx playwright test --headed
  ```

* **Abrir la interfaz interactiva (UI Mode):**
  ```bash
  npx playwright test --ui
  ```

* **Visualizar el reporte HTML de resultados:**
  ```bash
  npx playwright show-report
  ```

---

## 6. Evidencias y Reportes
Al finalizar cada ejecución, Playwright genera automáticamente:
* **Reporte HTML:** Disponible en `playwright-report/index.html`.
* **Capturas y trazas (*Traces*):** Guardadas en la carpeta `test-results/` en caso de fallo para depuración paso a paso.
