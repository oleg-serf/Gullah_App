# Migration `20200831181000-added-storefronts-and-products`

This migration has been generated by Dillon Raphael at 8/31/2020, 2:10:00 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "public"."Storefront" (
"id" SERIAL,
"createdAt" timestamp(3)   NOT NULL DEFAULT CURRENT_TIMESTAMP,
"updatedAt" timestamp(3)   NOT NULL ,
"name" text   NOT NULL ,
"description" text   NOT NULL ,
PRIMARY KEY ("id")
)

CREATE TABLE "public"."Product" (
"id" SERIAL,
"createdAt" timestamp(3)   NOT NULL DEFAULT CURRENT_TIMESTAMP,
"updatedAt" timestamp(3)   NOT NULL ,
"title" text   NOT NULL ,
"description" text   NOT NULL ,
"storefrontId" integer   NOT NULL ,
PRIMARY KEY ("id")
)

ALTER TABLE "public"."Product" ADD FOREIGN KEY ("storefrontId")REFERENCES "public"."Storefront"("id") ON DELETE CASCADE ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200827131727-init..20200831181000-added-storefronts-and-products
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
@@ -35,4 +35,23 @@
   antiCSRFToken      String?
   publicData         String?
   privateData        String?
 }
+
+model Storefront {
+  id          Int       @default(autoincrement()) @id
+  createdAt   DateTime  @default(now())
+  updatedAt   DateTime  @updatedAt
+  name        String    
+  description String    
+  products    Product[] 
+}
+
+model Product {
+  id           Int        @default(autoincrement()) @id
+  createdAt    DateTime   @default(now())
+  updatedAt    DateTime   @updatedAt
+  title        String     
+  description  String     
+  storefront   Storefront @relation(fields: [storefrontId], references: [id])
+  storefrontId Int        
+}
```

