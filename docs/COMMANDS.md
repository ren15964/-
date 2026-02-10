# 通用毕设：常用命令（Windows 友好）

## 后端（Maven / Spring Boot）

在项目根目录：

```powershell
cd smart-campus-backend
mvn -v
mvn clean package -DskipTests
mvn spring-boot:run
```

## 前端（Vue3 / Vite）

```powershell
cd smart-campus-frontend
node -v
npm -v

npm install
npm run dev
npm run build
npm run preview
```

## 数据库（MySQL）

```powershell
# 进入 mysql（按你的账号密码）
mysql -u root -p

# 在 mysql 里执行（示例）
source database.sql
```

> 也可用 Navicat / DBeaver 直接导入 `database.sql`。
