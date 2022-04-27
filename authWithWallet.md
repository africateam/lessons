### Step 1:

You need to expend you Backend API

Look `app.ts file`

### Step 2:

Create `module` folder

### Step 3: Create migrations

```json
export const db = {
    database: process.env.DB_NAME || 'backend_imara_dev',
    user: process.env.DB_USER || 'postgres',
    password: process.env.DB_PASSWORD || '[your password]',
    host: process.env.DB_HOST || '127.0.0.1',
    port: Number(process.env.DB_PORT) || 5432,

};
```
```json
export const tables = {
  USERS: {
    TABLE_NAME: "users",
    ID: "id",
    WALLET_ID: "walletId",
    PROFILE_PICTURE: "profilePicture",
    COUNTRY: "country",
    NAME: "name",
    SURNAME: "surname",
    COMPLETED: "completed",
    DESCRIPTION: "description",
    CREATED_AT: "createdAt",
  },
  JWTS: {
    TABLE_NAME: "jwts",
    ID: "id",
    WALLET_ID: "walletId",
    REFRESH_TOKEN: "refreshToken",
  },

};
```

### Create global settings

const path = require('path');

const APP_ROOT = __dirname + '/../..';
```
export const globals = 
{
    APP_PORT: process.env.PORT || 4002,
    APP_ROOT,
    PUBLIC_ROOT: path.join(APP_ROOT, 'public'),
    BASE_PATH: process.env.BASE_PATH || '',
    TEMP_DATA_PATH: path.join(APP_ROOT, 'temp' ),
};
```
