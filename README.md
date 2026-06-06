# 🧵 Prisma Schema — String Type Complete Guide

> **ডেটাবেস:** PostgreSQL | **ORM:** Prisma  
> **উদ্দেশ্য:** Prisma-তে `String` টাইপের সব ভেরিয়েন্ট এবং `Unsupported` টাইপ সহজ ভাষায় বোঝা

---

## 📋 সম্পূর্ণ Schema

```prisma
generator client {
  provider = "prisma-client-js"
  output   = "../app/generated/prisma"
}

datasource db {
  provider = "postgresql"
}

model User {
  id    Int    @id @default(autoincrement())

  col1  String                        // সাধারণ String (NOT NULL)
  col2  String?                       // Optional String (NULL হতে পারে)
  col3  String  @default("Bangladesh") // Default মান সহ
  col4  String  @db.VarChar(1000)     // সর্বোচ্চ ১০০০ ক্যারেক্টার
  col5  String  @db.Text              // বড় টেক্সট
  col6  String  @db.Char(10)          // ঠিক ১০ ক্যারেক্টার
  col7  String  @db.Citext            // Case-insensitive টেক্সট
  col8  String  @db.Inet              // IP Address
  col9  String  @db.VarBit(100)       // Variable-length bit string
  col10 String  @db.Bit(8)            // Fixed-length bit string
  col11 String  @db.Uuid              // UUID
  col12 String  @db.Xml               // XML ডেটা

  col13 Unsupported("cidr")?          // CIDR নেটওয়ার্ক রেঞ্জ
  col14 Unsupported("macaddr")?       // MAC Address (6-byte)
  col15 Unsupported("macaddr8")?      // MAC Address (8-byte)
  col16 Unsupported("tsvector")?      // Full-text search vector
  col17 Unsupported("tsquery")?       // Full-text search query
  col18 Unsupported("ltree")?         // Hierarchical path
  col19 Unsupported("hstore")?        // Key-Value store
}
```

---

## 🔍 প্রতিটি Column বিস্তারিত ব্যাখ্যা

### ✅ Prisma Native String Types

| Column | Type | PostgreSQL Type | বাংলা ব্যাখ্যা |
|--------|------|-----------------|----------------|
| `col1` | `String` | `TEXT` | সাধারণ string, NULL হবে না, কোনো সাইজ লিমিট নেই |
| `col2` | `String?` | `TEXT` | Optional — NULL থাকতে পারে (`?` মানেই nullable) |
| `col3` | `String @default("Bangladesh")` | `TEXT` | কোনো মান না দিলে `"Bangladesh"` বসবে |

---

### 🎯 `@db.xxx` — Database-Specific String Types

| Column | Prisma Attribute | PostgreSQL Type | সর্বোচ্চ সাইজ | বাংলা ব্যাখ্যা |
|--------|-----------------|-----------------|--------------|----------------|
| `col4` | `@db.VarChar(1000)` | `VARCHAR(1000)` | ১০০০ ক্যারেক্টার | নির্দিষ্ট লিমিটের মধ্যে variable-length text |
| `col5` | `@db.Text` | `TEXT` | সীমাহীন | বড় পরিমাণ টেক্সট (article, description) |
| `col6` | `@db.Char(10)` | `CHAR(10)` | ঠিক ১০ | সবসময় ১০ ক্যারেক্টার — কম হলে space দিয়ে পূরণ করে |

---

### 🔐 Special Purpose String Types

| Column | Prisma Attribute | PostgreSQL Type | বাংলা ব্যাখ্যা | উদাহরণ |
|--------|-----------------|-----------------|----------------|--------|
| `col7` | `@db.Citext` | `CITEXT` | Case-insensitive text — "Hello" এবং "hello" একই | username, email search |
| `col8` | `@db.Inet` | `INET` | IPv4 বা IPv6 আইপি অ্যাড্রেস | `192.168.1.1`, `::1` |
| `col9` | `@db.VarBit(100)` | `VARBIT(100)` | ১ থেকে ১০০ বিট পর্যন্ত variable bit string | `1011010` |
| `col10` | `@db.Bit(8)` | `BIT(8)` | ঠিক ৮ বিট — কম হলে error | `10110101` |
| `col11` | `@db.Uuid` | `UUID` | Universally Unique Identifier | `550e8400-e29b-41d4-a716-446655440000` |
| `col12` | `@db.Xml` | `XML` | XML ফরম্যাটের ডেটা | `<user><name>Wasim</name></user>` |

---

### ⚠️ `Unsupported()` Types

> Prisma সরাসরি এই টাইপগুলো সাপোর্ট করে না।  
> কিন্তু PostgreSQL-এ এগুলো ব্যবহার করা যায় — Prisma শুধু schema generate করবে, **Prisma Client-এ read/write সরাসরি করা যাবে না।**

| Column | Type | PostgreSQL Type | বাংলা ব্যাখ্যা | উদাহরণ |
|--------|------|-----------------|----------------|--------|
| `col13` | `Unsupported("cidr")?` | `CIDR` | নেটওয়ার্ক রেঞ্জ (host bits মুছে দেয়) | `192.168.0.0/24` |
| `col14` | `Unsupported("macaddr")?` | `MACADDR` | MAC Address (6-byte / EUI-48) | `08:00:2b:01:02:03` |
| `col15` | `Unsupported("macaddr8")?` | `MACADDR8` | MAC Address (8-byte / EUI-64) | `08:00:2b:ff:fe:01:02:03` |
| `col16` | `Unsupported("tsvector")?` | `TSVECTOR` | Full-text search-এর জন্য processed টেক্সট | `'bangl':1 'prisma':2` |
| `col17` | `Unsupported("tsquery")?` | `TSQUERY` | Full-text search query | `'prisma' & 'schema'` |
| `col18` | `Unsupported("ltree")?` | `LTREE` | Tree/hierarchy path | `Bangladesh.Dhaka.Mirpur` |
| `col19` | `Unsupported("hstore")?` | `HSTORE` | Key-value pair store | `"color"=>"red", "size"=>"L"` |

---

## 🆚 `String` vs `String?` পার্থক্য

```prisma
col1 String    // ❌ NULL দেওয়া যাবে না — সবসময় মান লাগবে
col2 String?   // ✅ NULL দেওয়া যাবে — মান না থাকলেও চলবে
```

| বিষয় | `String` | `String?` |
|------|---------|----------|
| NULL অনুমতি | ❌ না | ✅ হ্যাঁ |
| Database | `NOT NULL` constraint | NULL allowed |
| TypeScript Type | `string` | `string \| null` |

---

## 🆚 `VARCHAR` vs `TEXT` vs `CHAR` পার্থক্য

```
CHAR(10)     → সবসময় ১০ ক্যারেক্টার, কম হলে space যোগ হয়
VARCHAR(100) → ১ থেকে ১০০ ক্যারেক্টার, প্রয়োজন মতো জায়গা নেয়
TEXT         → যেকোনো দৈর্ঘ্যের টেক্সট, কোনো লিমিট নেই
```

| Type | সাইজ | Speed | Use Case |
|------|------|-------|----------|
| `CHAR(n)` | Fixed | দ্রুত | Fixed-length codes (ZIP, OTP) |
| `VARCHAR(n)` | Variable ≤ n | মাঝারি | Name, title, email |
| `TEXT` | Unlimited | মাঝারি | Article, description, logs |

---

## 🔎 Schema Validation — `npx prisma validate`

```bash
npx prisma validate
```

### এই command কী করে?

```
✔ Schema পড়ে এবং syntax error চেক করে
✔ Model, field, relation সব ঠিকঠাক আছে কিনা দেখে
✔ @db.xxx attributes database provider-এর সাথে compatible কিনা যাচাই করে
✔ Unsupported() types ঠিকমতো ব্যবহার হয়েছে কিনা চেক করে
```

### ✅ সফল হলে Output:

```
✔ The schema at "prisma/schema.prisma" is valid 🚀
```

### ❌ Error হলে Output (উদাহরণ):

```
Error validating field `col7` in model `User`:
Native type Citext of db is not supported for provider postgresql.
(Extension 'citext' must be enabled first)
```

> **⚠️ মনে রাখবে:** `@db.Citext` ব্যবহার করতে হলে PostgreSQL-এ `citext` extension আগে enable করতে হবে:
> ```sql
> CREATE EXTENSION IF NOT EXISTS citext;
> ```

---

## 🏃 পরবর্তী ধাপ — Migration করতে হলে

```bash
# Development-এ migration তৈরি করো
npx prisma migrate dev --name add_string_types

# Prisma Client generate করো
npx prisma generate

# Database-এ সরাসরি push করো (migration ছাড়া)
npx prisma db push
```

---

## 📌 Quick Reference Summary

```
String           → সাধারণ টেক্সট (NOT NULL)
String?          → Optional টেক্সট (NULL হতে পারে)
@default("...")  → Default মান সেট করে

@db.VarChar(n)  → সর্বোচ্চ n ক্যারেক্টার
@db.Text        → যেকোনো দৈর্ঘ্যের টেক্সট
@db.Char(n)     → ঠিক n ক্যারেক্টার (fixed)
@db.Citext      → Case-insensitive টেক্সট
@db.Inet        → IP Address
@db.VarBit(n)   → Variable bit string (≤ n bits)
@db.Bit(n)      → Fixed bit string (= n bits)
@db.Uuid        → UUID ফরম্যাট
@db.Xml         → XML ডেটা

Unsupported("cidr")     → CIDR নেটওয়ার্ক রেঞ্জ
Unsupported("macaddr")  → MAC Address (6-byte)
Unsupported("macaddr8") → MAC Address (8-byte)
Unsupported("tsvector") → Full-text search vector
Unsupported("tsquery")  → Full-text search query
Unsupported("ltree")    → Tree path (hierarchy)
Unsupported("hstore")   → Key-Value store
```

---

> 📝 **তৈরি করা হয়েছে:** Prisma + PostgreSQL প্র্যাকটিক্যাল শেখার জন্য
