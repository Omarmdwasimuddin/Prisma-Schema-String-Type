## Prisma-Schema-String-Type

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
}
