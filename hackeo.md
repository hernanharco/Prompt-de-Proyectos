# Prueba desde dashboard en la pagina authCore
```python
from fastapi import APIRouter, Depends, HTTPException
from app.core.security import get_current_user # Tu guardián de cookies

router = APIRouter()

@router.post("/update-homepage")
async def update_homepage(data: dict, current_user = Depends(get_current_user)):
    # 🚨 AQUÍ ESTÁ LA MAGIA: 
    # El backend verifica el ROL real que viene en el JWT de la COOKIE
    if current_user.role != "ADMIN":
        raise HTTPException(
            status_code=403, 
            detail="No tienes permisos de Admin. Tu hackeo de LocalStorage no funciona aquí."
        )
    
    # Si llega aquí, es un Admin real
    return {"status": "success", "message": "Contenido actualizado en MongoDB"}
```

## Prueba desde la terminal f12
```python
fetch('/backend/api/v1/content/update-homepage', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ info: "Probando seguridad final" }),
  credentials: 'include' // <--- Esto enviará tu cookie real
})
.then(async r => {
  const data = await r.json();
  console.log(`%cEstatus: ${r.status}`, "font-weight: bold; color: " + (r.status === 403 ? "orange" : "green"));
  console.log("Respuesta del Backend:", data.detail || data.message);
});
```
