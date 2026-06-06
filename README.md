## Prisma-Schema-String-Type

```bash
generator client {
  provider = "prisma-client-js"
  output   = "../app/generated/prisma"
}

datasource db {
  provider = "postgresql"
}

model User {
  id Int @id @default(autoincrement())

  col1 String
  col2 String?
  col3 String @default("Bangladesh")
  col4 String @db.VarChar(1000)
  col5 String @db.Text
  col6 String @db.Char(10)
  col7 String @db.Citext
  col8 String @db.Inet
  col9 String @db.VarBit(100)
  col10 String @db.Bit(8)
  col11 String @db.Uuid
  col12 String @db.Xml

  col13 Unsupported("cidr")?
  col14 Unsupported("macaddr")?
  col15 Unsupported("macaddr8")?
  col16 Unsupported("tsvector")?
  col17 Unsupported("tsquery")?
  col18 Unsupported("ltree")?
  col19 Unsupported("hstore")?
}
```
---


#### prisma schema er validation check koro
```bash
npx prisma validate
```
---
