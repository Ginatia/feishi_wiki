## 注册PostgreSQL 服务

注意使用管理员权限

```powershell
pg_ctl.exe register -N "dev" -D "D:\PSQL\data"
```

## 启动 PostgreSQL 服务

```powershell
pg_ctl.exe start -N "dev"
```

## 登录PostgreSQL

```powershell
psql -U postgres
```

