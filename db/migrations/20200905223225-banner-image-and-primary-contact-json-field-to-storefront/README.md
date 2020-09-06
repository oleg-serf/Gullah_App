# Migration `20200905223225-banner-image-and-primary-contact-json-field-to-storefront`

This migration has been generated by Dillon Raphael at 9/5/2020, 6:32:25 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
ALTER TABLE "public"."Storefront" ADD COLUMN "bannerImage" text   NOT NULL ,
ADD COLUMN "primaryContact" jsonb   NOT NULL 
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200905222108-create-categories-and-associations..20200905223225-banner-image-and-primary-contact-json-field-to-storefront
--- datamodel.dml
+++ datamodel.dml
@@ -1,10 +1,11 @@
 // This is your Prisma schema file,
 // learn more about it in the docs: https://pris.ly/d/prisma-schema
 datasource db {
-  provider = ["sqlite", "postgres"]
-  url = "***"
+  // provider = ["sqlite", "postgres"]
+  provider = ["postgres"]
+  url = "***"
 }
 generator client {
   provider = "prisma-client-js"
@@ -46,9 +47,11 @@
   description String    
   products    Product[]
   user        User @relation(fields: [userId], references: [id])
   userId      Int @unique
-  categories  Category[] 
+  categories  Category[]
+  bannerImage String
+  primaryContact Json 
 }
 model Product {
   id           Int        @default(autoincrement()) @id
```

