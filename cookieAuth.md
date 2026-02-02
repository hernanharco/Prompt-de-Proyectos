# Análisis Completo del Flujo de Autenticación con Google - AuthCore

Basado en el análisis detallado de tu código, aquí está el flujo completo de autenticación con Google enfocado en el manejo de cookies httpOnly:

## 1. Trigger: Click en Botón de Login con Google
Ruta: /frontend-authCore/src/components/AuthView.tsx > AuthView > loginGoogle()

```typescript
// Líneas 13-25
const loginGoogle = useGoogleLogin({
  onSuccess: async (tokenResponse) => {
    await auth.loginWithGoogle(tokenResponse.code);
  },
  flow: 'auth-code',
});
```

Ruta: /frontend-authCore/src/components/AuthView.tsx > AuthView > renderLoginForm() > onClick

```typescript
// Líneas 123-148
<button onClick={() => loginGoogle()}>
  Continuar con Google
</button>
```
## 2. Proxy de Red: Next.js hacia Backend
Ruta: /frontend-authCore/src/hooks/useAuth.ts > useAuth > loginWithGoogle()

```typescript
// Líneas 100-134
const response = await fetch(`${API_CONFIG.baseUrl}${API_CONFIG.endpoints.auth.google}`, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ token: code }),
  credentials: 'include', // ¡CLAVE! Esto permite que las cookies viajen
});
```

Ruta: /frontend-authCore/src/config/api.ts > API_CONFIG

```typescript
// Líneas 4-10
baseUrl: process.env.NEXT_PUBLIC_API_URL || '/backend',
endpoints: {
  google: '/api/v1/auth/google',
}
```

Ruta: /frontend-authCore/next.config.ts > nextConfig > rewrites()

```typescript
// Líneas 4-11
async rewrites() {
  return [
    {
      source: "/backend/:path*",
      destination: `${process.env.BACKEND_URL}/:path*`, 
    },
  ];
}
```

## 3. Backend Auth: Validación y JWT Generation
Ruta: /backend-authCore/app/api/v1/endpoints/auth.py > google_login()

```python
# Líneas 82-158
@router.post("/google", response_model=UserLoginResponse)
async def google_login(response: Response, *, db: Session = Depends(get_db), data: GoogleLogin):
    # 1. Validar token con Google
    flow = Flow.from_client_config(...)
    flow.fetch_token(code=data.token)
    id_info = id_token.verify_oauth2_token(...)
    
    # 2. Crear o encontrar usuario
    user = db.query(User).filter(User.email == email).first()
    
    # 3. Generar JWT propio
    access_token = create_access_token(subject=user.id)
    
    # 4. Setear cookie httpOnly
    set_auth_cookie(response, access_token)
```

## 4. Set-Cookie: Función Crítica de Seguridad
Ruta: /backend-authCore/app/api/v1/endpoints/auth.py > set_auth_cookie()

```python
# Líneas 30-47
def set_auth_cookie(response: Response, access_token: str):
    is_prod = settings.ENVIRONMENT == "production"
    
    response.set_cookie(
        key="access_token",
        value=access_token,
        httponly=True,           # ¡JavaScript NO puede acceder!
        secure=is_prod,          # HTTPS en producción
        samesite="lax" if not is_prod else "none",
        max_age=settings.ACCESS_TOKEN_EXPIRE_MINUTES * 60,
        path="/",
    )
```

## 5. Handshake de Regreso: Estrategia Híbrida
Ruta: /frontend-authCore/src/hooks/useAuth.ts > useAuth > setUser()

```typescript
// Líneas 37-44
const setUser = useCallback((user: User | null) => {
  setState(prev => ({ ...prev, user }));
  if (user) {
    localStorage.setItem('auth_user', JSON.stringify(user)); // Solo datos no sensibles
  } else {
    localStorage.removeItem('auth_user');
  }
}, []);
```
Ruta: /frontend-authCore/src/hooks/useAuth.ts > useAuth > loginWithGoogle()

```typescript
// Líneas 119-125
setUser(successData.user);  // Guarda datos públicos en localStorage
setSuccess('Conectado con Google con éxito');
 
if (typeof window !== 'undefined') {
  window.location.href = '/dashboard';  // Redirección hard refresh
}
```

## 6. Middleware de Acceso: Validación de Cookies
Ruta: /backend-authCore/app/core/security.py > OAuth2PasswordBearerWithCookie > __call__()

```python
# Líneas 19-33
class OAuth2PasswordBearerWithCookie(OAuth2PasswordBearer):
    async def __call__(self, request: Request) -> Optional[str]:
        # 1. Buscar token en cookie httpOnly
        token: str = request.cookies.get("access_token")
        
        # 2. Fallback a Authorization header
        if not token:
            token = await super().__call__(request)
            
        return token
```
Ruta: /backend-authCore/app/core/security.py > get_current_user()

``python
# Líneas 81-109
def get_current_user(token: str = Depends(oauth2_scheme), db: Session = Depends(get_db)) -> User:
    # 1. Decodificar JWT
    payload = jwt.decode(token, settings.SECRET_KEY, algorithms=[settings.ALGORITHM])
    user_id: str = payload.get("sub")
    
    # 2. Validar en BD
    user = db.query(User).filter(User.id == user_id).first()
    return user
```

## 7. Ejemplo: Acceso a Dashboard Protegido
Ruta: /backend-authCore/app/api/v1/endpoints/users.py > read_user_me()

python
# Líneas 94-98
@router.get("/me", response_model=UserResponse)
async def read_user_me(current_user: User = Depends(get_current_active_user)):
    return current_user
Flujo de Seguridad por Defecto
Cookie httpOnly: JavaScript del cliente nunca puede acceder al token JWT
Credentials: include: El navegador automáticamente incluye la cookie en cada petición
OAuth2PasswordBearerWithCookie: Extrae token de cookie primero, luego de header
LocalStorage híbrido: Solo guarda datos públicos (nombre, email), no el token
Redirección hard refresh: Asegura que la cookie se establezca correctamente antes de navegar
Este diseño cumple con tu principio de "Seguridad por Defecto" manteniendo el token JWT completamente inaccesible para el JavaScript del cliente mientras permite una experiencia de usuario fluida.
