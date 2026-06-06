# 🧵 Prisma Schema — String Type (Line by Line ব্যাখ্যা)

> **ডেটাবেস:** PostgreSQL | **ORM:** Prisma  
> **লক্ষ্য:** প্রতিটি লাইন কী করে, কেন লেখা হয় — সেটা বোঝা

---

## 📦 Model ডিফাইন করা

```prisma
model User {
```

> 📌 এটি একটি ডাটাবেস **টেবিল** ডিফাইন করছে।  
> টেবিলের নাম হবে **`User`**।  
> এই ব্লকের ভেতরে যা লেখা হবে, সেগুলো হবে সেই টেবিলের **কলাম**।

---

## 🔑 Primary Key

```prisma
  id Int @id @default(autoincrement())
```

| অংশ | মানে |
|-----|------|
| `id` | কলামের নাম |
| `Int` | ডাটা টাইপ — পূর্ণসংখ্যা (Integer) |
| `@id` | এটি টেবিলের **Primary Key** — প্রতিটি row-এর জন্য ইউনিক |
| `@default(autoincrement())` | নতুন row যোগ হলে অটোমেটিক ১, ২, ৩ করে বাড়বে |

---

## 🔤 String Type — এক এক করে

---

### ১. সাধারণ String (NOT NULL)

```prisma
  col1 String
```

> 📌 **`String`** — PostgreSQL-এ এটি `TEXT` টাইপ হিসেবে সেভ হয়।  
> আনলিমিটেড সাইজের টেক্সট নিতে পারে।  
> এখানে **NULL দেওয়া যাবে না** — মান দিতেই হবে।

---

### ২. Optional String (NULL হতে পারে)

```prisma
  col2 String?
```

> 📌 **`?`** চিহ্নটির মানে — এই কলামটি **Optional (Nullable)**।  
> অর্থাৎ ডাটা না দিলেও চলবে, `null` থাকতে পারে।  
> TypeScript-এ এটি `string | null` হিসেবে আসবে।

```
String  → NOT NULL  (মান দিতেই হবে)
String? → NULLABLE  (না দিলেও চলবে)
```

---

### ৩. Default মান সহ String

```prisma
  col3 String @default("Bangladesh")
```

> 📌 **`@default("Bangladesh")`** — ডাটা insert করার সময় এই কলামে কিছু না দিলে  
> অটোমেটিক **`"Bangladesh"`** বসে যাবে।

---

### ৪. VarChar — সর্বোচ্চ সাইজ সীমিত

```prisma
  col4 String @db.VarChar(1000)
```

> 📌 **`@db.VarChar(1000)`** — PostgreSQL-এর `varchar` টাইপ।  
> সর্বোচ্চ **১০০০ ক্যারেক্টার** পর্যন্ত নিতে পারবে।  
> ১০০০-এর বেশি দিলে **error** দেবে।

---

### ৫. Text — বড় টেক্সটের জন্য

```prisma
  col5 String @db.Text
```

> 📌 **`@db.Text`** — স্পষ্টভাবে `text` টাইপ বলে দেওয়া হচ্ছে।  
> আনলিমিটেড টেক্সট নিতে পারে।  
> ব্লগ পোস্ট, বিবরণ, লম্বা লেখার জন্য উপযুক্ত।  
> *(Prisma-তে শুধু `String` লিখলেও আসলে এটাই হয়)*

---

### ৬. Char — Fixed Length

```prisma
  col6 String @db.Char(10)
```

> 📌 **`@db.Char(10)`** — Fixed Length ক্যারেক্টার।  
> সবসময় **ঠিক ১০টি** ক্যারেক্টার থাকবে।  
> যদি `"BD"` (২ অক্ষর) দাও → বাকি ৮টি জায়গা **space দিয়ে পূরণ** হবে।

```
"BD" → "BD        "  (৮টি space যোগ হয়)
```

---

### ৭. Citext — Case-Insensitive

```prisma
  col7 String @db.Citext
```

> 📌 **`@db.Citext`** — Case-Insensitive Text।  
> `"ABC"` আর `"abc"` ডাটাবেস **একই** মনে করবে।  
> Email বা Username search-এর জন্য খুব কাজের।

```sql
-- এই দুটো query একই result দেবে:
WHERE email = 'Wasim@gmail.com'
WHERE email = 'wasim@gmail.com'
```

> ⚠️ ব্যবহার করতে হলে আগে PostgreSQL-এ extension চালু করতে হবে:
> ```sql
> CREATE EXTENSION IF NOT EXISTS citext;
> ```

---

### ৮. Inet — IP Address

```prisma
  col8 String @db.Inet
```

> 📌 **`@db.Inet`** — IPv4 বা IPv6 আইপি অ্যাড্রেস সেভ করার জন্য।  
> সাধারণ String-এ IP রাখলে validation নেই, কিন্তু `Inet`-এ ডাটাবেস নিজেই চেক করে।

```
192.168.1.1        ✅ Valid IPv4
::1                ✅ Valid IPv6
999.999.999.999    ❌ Error — invalid IP
```

---

### ৯. VarBit — Variable Bit String

```prisma
  col9 String @db.VarBit(100)
```

> 📌 **`@db.VarBit(100)`** — পরিবর্তনশীল দৈর্ঘ্যের **Bit String**।  
> সর্বোচ্চ **১০০ বিট** পর্যন্ত হতে পারে।  
> Binary permission, flag বা bitmask রাখতে কাজে আসে।

```
"101"          ✅ (৩ বিট)
"10110010"     ✅ (৮ বিট)
```

---

### ১০. Bit — Fixed Bit String

```prisma
  col10 String @db.Bit(8)
```

> 📌 **`@db.Bit(8)`** — Fixed **৮ বিটের** ডাটা।  
> একদম ৮ বিট দিতে হবে — কম বা বেশি হলে **error**।

```
"10110101"   ✅ ঠিক ৮ বিট — OK
"101"        ❌ মাত্র ৩ বিট — Error
```

---

### ১১. UUID

```prisma
  col11 String @db.Uuid
```

> 📌 **`@db.Uuid`** — Universally Unique Identifier।  
> এটি একটি বিশেষ ফরম্যাটের ইউনিক আইডি।  
> Public API-তে id expose করতে হলে এটি ব্যবহার করা নিরাপদ।

```
550e8400-e29b-41d4-a716-446655440000   ✅ Valid UUID
```

---

### ১২. XML

```prisma
  col12 String @db.Xml
```

> 📌 **`@db.Xml`** — XML ফরম্যাটের ডাটা সেভ করার জন্য।  
> ডাটাবেস XML structure validate করে।

```xml
<user><name>Wasim</name><city>Dhaka</city></user>
```

---

## ⚠️ Unsupported Types

> Prisma এই টাইপগুলো **সরাসরি চেনে না**।  
> কিন্তু PostgreSQL-এ এগুলো বিদ্যমান।  
> **`Unsupported("type_name")`** লিখলে Prisma schema-তে রাখবে,  
> তবে **Prisma Client দিয়ে সরাসরি read/write করা যাবে না** — raw SQL লাগবে।

---

### ১৩. CIDR — নেটওয়ার্ক রেঞ্জ

```prisma
  col13 Unsupported("cidr")?
```

> 📌 নেটওয়ার্ক আইপি মাস্ক রাখার জন্য।  
> `Inet` IP-র host bits রাখে, কিন্তু `CIDR` সেগুলো মুছে দেয়।

```
192.168.0.0/24   ✅ CIDR format
```

---

### ১৪. MACADDR — MAC Address (6-byte)

```prisma
  col14 Unsupported("macaddr")?
```

> 📌 হার্ডওয়্যারের **MAC Address** স্টোর করার জন্য।  
> নেটওয়ার্ক ডিভাইস ট্র্যাকিং বা লগিং-এ ব্যবহার হয়।

```
08:00:2b:01:02:03   ✅ EUI-48 format (6-byte)
```

---

### ১৫. MACADDR8 — MAC Address (8-byte)

```prisma
  col15 Unsupported("macaddr8")?
```

> 📌 নতুন **৮-বাইটের MAC Address** (EUI-64) স্টোর করার জন্য।

```
08:00:2b:ff:fe:01:02:03   ✅ EUI-64 format (8-byte)
```

---

### ১৬. TSVECTOR — Full-Text Search Vector

```prisma
  col16 Unsupported("tsvector")?
```

> 📌 **Full Text Search**-এর জন্য optimized টেক্সট।  
> PostgreSQL টেক্সটকে প্রসেস করে এমনভাবে রাখে যাতে বড় ডাটায় খুব দ্রুত সার্চ করা যায়।

```
'bangl':1 'prisma':2 'schema':3   ← tsvector ফরম্যাট
```

---

### ১৭. TSQUERY — Full-Text Search Query

```prisma
  col17 Unsupported("tsquery")?
```

> 📌 Full-text search-এর **সার্চ শর্ত** রাখার জন্য।  
> `tsvector`-এর সাথে মিলিয়ে ব্যবহার করা হয়।

```
'prisma' & 'schema'   ← AND condition
'prisma' | 'orm'      ← OR condition
```

---

### ১৮. LTREE — Hierarchical Path

```prisma
  col18 Unsupported("ltree")?
```

> 📌 Tree বা hierarchy স্ট্রাকচারের ডাটা রাখার জন্য।  
> ফোল্ডার স্ট্রাকচার, ক্যাটাগরি tree, অর্গানাইজেশন চার্ট-এ ব্যবহার হয়।

```
Bangladesh.Dhaka.Mirpur     ✅ Hierarchy path
Top.Science.Physics         ✅ Category tree
```

---

### ১৯. HSTORE — Key-Value Store

```prisma
  col19 Unsupported("hstore")?
```

> 📌 **Key-Value পেয়ার** স্টোর করার জন্য।  
> এটি পুরাতন টাইপ — বর্তমানে এর পরিবর্তে **`Json`** বেশি ব্যবহার করা হয়।

```
"color"=>"blue", "size"=>"XL"   ← hstore format
{"color": "blue", "size": "XL"} ← JSON format (আধুনিক)
```

---

## ✅ Schema Validate করো

```bash
npx prisma validate
```

> সফল হলে দেখাবে:
> ```
> ✔ The schema at "prisma/schema.prisma" is valid 🚀
> ```

---

## 📌 সব Type এক নজরে

| Column | Prisma Type | PostgreSQL Type | সহজ ব্যাখ্যা |
|--------|------------|-----------------|--------------|
| `col1` | `String` | `TEXT` | সাধারণ string, NOT NULL |
| `col2` | `String?` | `TEXT` | NULL হতে পারে |
| `col3` | `@default("Bangladesh")` | `TEXT` | Default মান |
| `col4` | `@db.VarChar(1000)` | `VARCHAR(1000)` | সর্বোচ্চ ১০০০ অক্ষর |
| `col5` | `@db.Text` | `TEXT` | আনলিমিটেড টেক্সট |
| `col6` | `@db.Char(10)` | `CHAR(10)` | ঠিক ১০ অক্ষর |
| `col7` | `@db.Citext` | `CITEXT` | Case-insensitive |
| `col8` | `@db.Inet` | `INET` | IP Address |
| `col9` | `@db.VarBit(100)` | `VARBIT(100)` | Variable bit (≤ 100) |
| `col10` | `@db.Bit(8)` | `BIT(8)` | Fixed 8-bit |
| `col11` | `@db.Uuid` | `UUID` | Unique ID |
| `col12` | `@db.Xml` | `XML` | XML ডেটা |
| `col13` | `Unsupported("cidr")` | `CIDR` | Network range |
| `col14` | `Unsupported("macaddr")` | `MACADDR` | MAC Address 6-byte |
| `col15` | `Unsupported("macaddr8")` | `MACADDR8` | MAC Address 8-byte |
| `col16` | `Unsupported("tsvector")` | `TSVECTOR` | Search vector |
| `col17` | `Unsupported("tsquery")` | `TSQUERY` | Search query |
| `col18` | `Unsupported("ltree")` | `LTREE` | Tree path |
| `col19` | `Unsupported("hstore")` | `HSTORE` | Key-Value pairs |

---

> 📝 **তৈরি করা হয়েছে:** Prisma + PostgreSQL প্র্যাকটিক্যাল শেখার জন্য
