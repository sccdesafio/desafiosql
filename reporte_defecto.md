# Reporte de Defecto (Bug Report)

## BUG-01: El campo "Last Name" sobrescribe el campo "First Name" e impide completar el Checkout con `problem_user`

| Campo | Detalle |
| :--- | :--- |
| **ID del Defecto** | `BUG-01` |
| **Título Descriptivo** | Bloqueo en formulario de Checkout: escribir en "Last Name" borra/sobrescribe el campo "First Name" y arroja error de validación (`Last Name is required`) |
| **Severidad** | **Crítica (Critical)** — Bloquea por completo el flujo principal de compra (Checkout) impidiendo generar órdenes. |
| **Prioridad** | **Alta (High)** — Debe resolverse de inmediato, ya que afecta la conversión y el proceso de pago en producción. |
| **Entorno** | • **URL:** `https://saucedemo.com`<br>• **Usuario de prueba:** `problem_user` / `secret_sauce`<br>• **Navegador:** Chromium v130.0 (Playwright) / Google Chrome<br>• **Sistema Operativo:** Windows 11 (64-bit) |

---

### Pasos para reproducir
1. Ingresar a `https://saucedemo.com`.
2. Iniciar sesión con las credenciales del usuario con defectos:
   * **Username:** `problem_user`
   * **Password:** `secret_sauce`
3. En el catálogo de productos (`/inventory.html`), hacer clic en **Add to cart** en cualquier artículo (ej. *Sauce Labs Backpack*).
4. Hacer clic en el ícono del carrito de compras en la esquina superior derecha (`/cart.html`).
5. Hacer clic en el botón **Checkout** (`/checkout-step-one.html`).
6. Ingresar un nombre válido en el campo **First Name** (ej. `Juan`).
7. Hacer clic en el campo **Last Name** e intentar escribir un apellido (ej. `Pérez`).
8. Ingresar un código postal en **Zip/Postal Code** (ej. `7500000`) y hacer clic en **Continue**.

---

### Resultado Actual
* Al escribir en el campo **Last Name**, los caracteres ingresados no se escriben en dicho campo, sino que sobrescriben el texto previamente ingresado en **First Name**, dejando el campo **Last Name** vacío.
* Al presionar **Continue**, el sistema bloquea el avance y muestra el mensaje de error:  
  `Error: Last Name is required`

---

### Resultado Esperado
* Cada campo del formulario (**First Name**, **Last Name** y **Zip/Postal Code**) debe recibir y conservar de forma independiente los datos ingresados por el usuario.
* Al presionar **Continue** con los tres campos completos, el sistema debe avanzar correctamente al resumen de la orden (`/checkout-step-two.html`).

---

### Evidencia (Captura / Traza)
* **Captura de pantalla:**  
  ![Evidencia del defecto en Checkout con problem_user](images/bug_checkout_problem_user.png)
* **Traza de ejecución (Playwright Trace):**  
  Disponible en `test-results/checkout-problem-user/trace.zip` (se observa la falla en la aserción `expect(page).toHaveURL(/checkout-step-two.html/)` tras el clic en el botón `#continue`).

