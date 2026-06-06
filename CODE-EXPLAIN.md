  model User {
   * ব্যাখ্যা: এটি একটি ডাটাবেস টেবিল ডিফাইন করছে যার নাম হবে User।

  ---

      id Int @id @default(autoincrement())
   * id Int: এটি একটি পূর্ণসংখ্যা (Integer) টাইপ কলাম।
   * @id: এটি দিয়ে বোঝানো হচ্ছে যে এটি টেবিলের Primary Key (অর্থাৎ প্রতিটি ইউজারের জন্য এই মানটি ইউনিক হবে)।
   * @default(autoincrement()): নতুন ডাটা ইনসার্ট করার সময় এই আইডিটি অটোমেটিক ১, ২, ৩ এভাবে বাড়বে।

  ---

      col1 String
   * String: এটি PostgreSQL-এ ডিফল্টভাবে text টাইপ হিসেবে সেভ হয়। এটি আনলিমিটেড সাইজের টেক্সট নিতে পারে।

  ---

      col2 String?
   * String?: এখানে ? চিহ্নটির মানে হলো এই কলামটি Optional বা Nullable। অর্থাৎ এখানে ডাটা না থাকলেও চলবে (null থাকতে পারে)।

  ---

      col3 String @default("Bangladesh")
   * @default("Bangladesh"): যদি ডাটা ইনসার্ট করার সময় আপনি এই কলামে কিছু না দেন, তবে ডাটাবেসে অটোমেটিক "Bangladesh" লেখাটি বসে যাবে।

  ---

      col4 String @db.VarChar(1000)
   * @db.VarChar(1000): এটি PostgreSQL-এর varchar টাইপ। এটি সর্বোচ্চ ১০০০ ক্যারেক্টার পর্যন্ত টেক্সট নিতে পারবে।

  ---

      col5 String @db.Text
   * @db.Text: এটি স্পষ্টভাবে বলে দিচ্ছে যে এটি বড় টেক্সট ডাটা রাখার জন্য text টাইপ ব্যবহার করবে (Prisma-তে String দিলে এমনিতে এটাই হয়)।

  ---

      col6 String @db.Char(10)
   * @db.Char(10): এটি Fixed Length ক্যারেক্টার টাইপ। এখানে সবসময় ১০টি ক্যারেক্টার থাকবে। যদি আপনি ২ অক্ষরের কিছু দেন, বাকি ৮টি ঘর খালি জায়গা (space) দিয়ে পূরণ হবে।

  ---

      col7 String @db.Citext
   * @db.Citext: এটি Case-Insensitive text। মানে আপনি "ABC" আর "abc" লিখে সার্চ করলে ডাটাবেস দুটিকে একই ধরবে। এটি ইমেইল বা ইউজারনেমের জন্য খুব কাজের।

  ---

      col8 String @db.Inet
   * @db.Inet: এটি IPv4 বা IPv6 এড্রেস সেভ করার জন্য ব্যবহৃত হয়।

  ---

   1   col9 String @db.VarBit(100)
   * @db.VarBit(100): এটি পরিবর্তনশীল দৈর্ঘ্যের বিট স্ট্রিং (যেমন: "0101") যা সর্বোচ্চ ১০০ বিট পর্যন্ত হতে পারে।

  ---

      col10 String @db.Bit(8)
   * @db.Bit(8): এটি ফিক্সড ৮ বিটের ডাটা নিবে। একদম ৮ বিট না হলে ডাটাবেস এরর দিবে।

  ---

      col11 String @db.Uuid
   * @db.Uuid: এটি একটি ইউনিক আইডি ফরম্যাট (যেমন: 550e8400-e29b...) স্টোর করার জন্য ব্যবহৃত হয়।

  ---

      col12 String @db.Xml
   * @db.Xml: এটি XML ডাটা সেভ করার জন্য ব্যবহৃত হয়।

  ---

      col13 Unsupported("cidr")?
   * Unsupported("cidr"): এটি নেটওয়ার্ক আইপি মাস্ক (IP Network/Mask) রাখার জন্য PostgreSQL-এর একটি স্পেশাল টাইপ যা Prisma সরাসরি চেনে না। তাই Unsupported ব্যবহার করা হয়েছে।

  ---

      col14 Unsupported("macaddr")?
   * macaddr: এটি হার্ডওয়্যারের MAC address (যেমন: 08:00:2b:01:02:03) স্টোর করার জন্য।

  ---

      col15 Unsupported("macaddr8")?
   * macaddr8: এটি নতুন ৮-বাইটের MAC address স্টোর করার জন্য।

  ---

     col16 Unsupported("tsvector")?
   * tsvector: এটি Full Text Search-এর জন্য ব্যবহৃত হয়। এটি টেক্সটকে এমনভাবে অপ্টিমাইজ করে যাতে খুব দ্রুত বড় ডাটার মধ্যে সার্চ করা যায়।

  ---

      col17 Unsupported("tsquery")?
   * tsquery: এটি সার্চ করার কোয়েরি বা শর্তগুলো রাখার জন্য ব্যবহৃত হয়।

  ---

      col18 Unsupported("ltree")?
   * ltree: এটি দিয়ে হায়ারার্কিক্যাল ডাটা (যেমন: ফোল্ডার স্ট্রাকচার: Top.Science.Physics) রাখা হয়।

  ---

      col19 Unsupported("hstore")?
   * hstore: এটি Key-Value পেয়ার (যেমন: "color"=>"blue") স্টোর করার জন্য একটি পুরাতন টাইপ। বর্তমানে মানুষ এর বদলে Json বেশি ব্যবহার করে।
