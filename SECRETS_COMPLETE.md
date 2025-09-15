# 🔐 Configuración Completa de Secrets para GitHub Actions

## Secrets Requeridos (Basado en tu configuración actual)

### Secrets que ya tienes configurados ✅
1. **AWS_ACCESS_ID** - ID de acceso de AWS
2. **AWS_ACCESS_KEY** - Clave secreta de AWS  
3. **EC2_HOST** - DNS público de la instancia EC2
4. **EC2_INSTANCE** - ID de la instancia EC2
5. **EC2_SSH_PRIVATE_KEY** - Clave privada SSH para EC2
6. **EC2_USER** - Usuario para conectarse a EC2

### Secrets Adicionales Recomendados 🔧

#### 7. **POSTGRES_ADMIN_PASSWORD** (Opcional pero recomendado)
- **Descripción**: Contraseña para el usuario administrador de PostgreSQL
- **Valor sugerido**: `postgres123`
- **Uso**: Para configurar la contraseña del usuario postgres

#### 8. **DB_PASSWORD** (Opcional pero recomendado)
- **Descripción**: Contraseña para el usuario de la aplicación
- **Valor sugerido**: `D1ymf8wyQEGthFR1E9xhCq`
- **Uso**: Para la variable de entorno DB_PASSWORD

#### 9. **DB_USER** (Opcional pero recomendado)
- **Descripción**: Usuario de la base de datos de la aplicación
- **Valor sugerido**: `LTIdbUser`
- **Uso**: Para la variable de entorno DB_USER

#### 10. **DB_NAME** (Opcional pero recomendado)
- **Descripción**: Nombre de la base de datos
- **Valor sugerido**: `LTIdb`
- **Uso**: Para la variable de entorno DB_NAME

## Cómo Agregar Secrets Adicionales

1. Ve a tu repositorio en GitHub
2. Click en **Settings** → **Secrets and variables** → **Actions**
3. Click en **New repository secret**
4. Agrega cada secret con su nombre y valor correspondiente

## Verificación de Secrets

Para verificar que todos los secrets están configurados:

```bash
# En el pipeline, puedes verificar que los secrets existen:
echo "EC2_HOST: ${{ secrets.EC2_HOST != '' }}"
echo "EC2_USER: ${{ secrets.EC2_USER != '' }}"
echo "EC2_SSH_PRIVATE_KEY: ${{ secrets.EC2_SSH_PRIVATE_KEY != '' }}"
```

## Troubleshooting de Secrets

Si el pipeline falla por problemas de autenticación:

1. **Verifica que EC2_SSH_PRIVATE_KEY** contenga TODO el archivo .pem (incluyendo las líneas BEGIN/END)
2. **Verifica que EC2_HOST** sea el DNS público correcto de tu instancia
3. **Verifica que EC2_USER** sea el usuario correcto (típicamente `ec2-user` para Amazon Linux)
4. **Verifica que la instancia EC2** esté en estado "Running"
5. **Verifica que las Security Groups** permitan SSH en el puerto 22

## Comandos de Verificación en EC2

```bash
# Verificar que el usuario existe
id ${{ secrets.EC2_USER }}

# Verificar permisos del directorio home
ls -la /home/${{ secrets.EC2_USER }}

# Verificar que PostgreSQL está funcionando
sudo systemctl status postgresql

# Verificar conectividad SSH
ssh -o ConnectTimeout=10 ${{ secrets.EC2_USER }}@${{ secrets.EC2_HOST }} "echo 'SSH OK'"
```
