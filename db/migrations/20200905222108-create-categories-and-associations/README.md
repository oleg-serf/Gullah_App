# Migration `20200905222108-create-categories-and-associations`

This migration has been generated by Dillon Raphael at 9/5/2020, 6:21:08 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "public"."Category" (
"id" SERIAL,
"createdAt" timestamp(3)   NOT NULL DEFAULT CURRENT_TIMESTAMP,
"updatedAt" timestamp(3)   NOT NULL ,
"title" text   NOT NULL ,
"parent" boolean   NOT NULL ,
"parentId" integer   NOT NULL ,
PRIMARY KEY ("id")
)

CREATE TABLE "public"."_CategoryToStorefront" (
"A" integer   NOT NULL ,
"B" integer   NOT NULL 
)

CREATE TABLE "public"."_CategoryToProduct" (
"A" integer   NOT NULL ,
"B" integer   NOT NULL 
)

CREATE UNIQUE INDEX "_CategoryToStorefront_AB_unique" ON "public"."_CategoryToStorefront"("A", "B")

CREATE INDEX "_CategoryToStorefront_B_index" ON "public"."_CategoryToStorefront"("B")

CREATE UNIQUE INDEX "_CategoryToProduct_AB_unique" ON "public"."_CategoryToProduct"("A", "B")

CREATE INDEX "_CategoryToProduct_B_index" ON "public"."_CategoryToProduct"("B")

ALTER TABLE "public"."_CategoryToStorefront" ADD FOREIGN KEY ("A")REFERENCES "public"."Category"("id") ON DELETE CASCADE ON UPDATE CASCADE

ALTER TABLE "public"."_CategoryToStorefront" ADD FOREIGN KEY ("B")REFERENCES "public"."Storefront"("id") ON DELETE CASCADE ON UPDATE CASCADE

ALTER TABLE "public"."_CategoryToProduct" ADD FOREIGN KEY ("A")REFERENCES "public"."Category"("id") ON DELETE CASCADE ON UPDATE CASCADE

ALTER TABLE "public"."_CategoryToProduct" ADD FOREIGN KEY ("B")REFERENCES "public"."Product"("id") ON DELETE CASCADE ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200902145251-make-userid-unique-on-storefront..20200905222108-create-categories-and-associations
--- datamodel.dml
+++ datamodel.dml
@@ -2,9 +2,9 @@
 // learn more about it in the docs: https://pris.ly/d/prisma-schema
 datasource db {
   provider = ["sqlite", "postgres"]
-  url = "***"
+  url = "***"
 }
 generator client {
   provider = "prisma-client-js"
@@ -45,9 +45,10 @@
   name        String    
   description String    
   products    Product[]
   user        User @relation(fields: [userId], references: [id])
-  userId      Int @unique 
+  userId      Int @unique
+  categories  Category[] 
 }
 model Product {
   id           Int        @default(autoincrement()) @id
@@ -55,6 +56,18 @@
   updatedAt    DateTime   @updatedAt
   title        String     
   description  String     
   storefront   Storefront @relation(fields: [storefrontId], references: [id])
-  storefrontId Int        
+  storefrontId Int
+  categories   Category[]        
 }
+
+model Category {
+  id        Int      @default(autoincrement()) @id
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+  title     String
+  parent    Boolean
+  parentId  Int
+  stores    Storefront[]
+  products  Product[]
+}
```


